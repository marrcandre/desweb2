# Tutorial — Django REST Framework (material futuro)

> **Status:** material de planejamento e referência. Ainda **não** é um conjunto de aulas
> completo. Este documento registra a **sequência didática proposta** e as **decisões já
> acordadas**, para que o tutorial DRF possa ser retomado e escrito mais adiante — seguindo a mesma
> filosofia das Aulas 2–13 do README (Express × FastAPI), mas com o **Django REST Framework** como
> terceira implementação do mesmo contrato HTTP.

Este documento **não altera** o repositório `django-bsi4`. O `django-bsi4` já existe e contém uma
implementação funcional (ver `django-bsi4/README.md`). O que propomos aqui é usá-lo como **ponto de
partida** ou **referência** para um futuro tutorial pedagógico.

---

## 1. Objetivo

Mostrar como a **mesma API** de produtos, já construída no Express e no FastAPI, ganha uma terceira
implementação em **Django + Django REST Framework (DRF)**. O contrato HTTP descrito na seção 8 do
`README.md` continua valendo:

- Base: `/api/produtos/`
- `GET` coleção, `GET` por ID, `POST`, `PUT`, `DELETE`
- Validação de `nome` (string, trim, 2–100) e `preco` (>0, ≤2 casas)
- Filtros `preco_minimo` / `preco_maximo`, busca `search`, ordenação `ordering`
- Erros em `{"detail": ...}` ou `{"detail": {campo: msg}}`

No DRF, muito desse contrato é obtido de forma **declarativa**, reduzindo o código manual.

---

## 2. Diferenças conceituais que o DRF introduz

Enquanto o Express e o FastAPI mantêm os dados **em memória / arquivo JSON** e validam à mão, o
Django introduz camadas que o aluno ainda não viu nas aulas 2–13:

| Camada | Papel |
| ------ | ----- |
| **Model (ORM)**   | a estrutura da tabela e a persistência em banco (SQLite) |
| **Migration**     | versiona a estrutura do banco |
| **Serializer**    | converte objeto ↔ JSON e valida os campos |
| **ViewSet/View**  | expõe as operações (CRUD) |
| **Router**        | monta as URLs automaticamente |
| **FilterBackend/Search/Ordering** | filtros, busca e ordenação |

É a mesma ideia de sempre: **o framework muda a forma de implementar, o contrato HTTP permanece**.

---

## 3. Sequência didática proposta

> Proposta de ordem das aulas DRF. Cada passo retoma um conceito já conhecido do Express/FastAPI e
> mostra como o DRF o expressa. Números são sugeridos e podem ser ajustados.

1. **Ambiente e projeto** — projeto Django + app `produtos` + `INSTALLED_APPS` (DRF) +
   `requirements.txt`; `runserver` básico.
2. **Model `Produto`** — `Model(produto)` com os campos; `makemigrations`/`migrate`; entender a
   tabela e o banco.
3. **CSRF/Admin opcional** — **não** foco central agora.
4. **Primeiro endpoint de leitura** — `ListAPIView` + `GET /api/produtos/` devolvendo o JSON;
   contraste com o `aula2` do Express.
5. **CRUD completo — `ModelViewSet` + `Router`** — `GET` coleção/ID, `POST`, `PUT`, `DELETE` em
   poucas linhas, seção do Express 4–6.
6. **Serializers e validação** — customização (nome 2–100, trim) em vez das funções manuais
   `validarProduto` (contraparte da Aula 7).
7. **Filtros** — `DjangoFilterBackend` + `ProdutoFilter` (`preco_minimo`/`preco_maximo`);
   contraparte da Aula 8.
8. **Busca** — `SearchFilter` (`search` em `nome`); contraparte da Aula 9.
9. **Ordenação** — `OrderingFilter` (`-preco`); contraparte da Aula 10.
10. **Persistência real** — o banco já persistenta; comparar com `produtos.json`.
11. **Paginação** — `PageNumberPagination` provendo `{count, next, previous, results}`; comparar
    com `{page, page_size, total_pages, results}` do README.
12. **Documentação automática** — `drf-spectacular` em `/api/schema/`, `/api/docs/` (Swagger),
    `/api/redoc/`.
13. **Consolidação** — quadro DRF × Express × FastAPI, sem conceito novo.

---

## 4. Decisões já acordadas (base real)

O `django-bsi4` atual é uma **referência** para esse futuro tutorial. Registramos aqui o que já foi
discutido, reproduzido fielmente do código existente, para não precisar re-descobrir depois:

### 4.1 Estrutura das URLs (`config/urls.py`)

```python
# router registra "categorias" e "produtos" sob /api/
router = DefaultRouter()
router.register(r"produtos", ProdutoViewSet)

urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/", include(router.urls)),
    path("api/schema/", SpectacularAPIView.as_view(), name="schema"),
    path("api/docs/", SpectacularSwaggerView.as_view(url_name="schema"), name="swagger-ui"),
    path("api/redoc/", SpectacularRedocView.as_view(url_name="schema"), name="redoc"),
]
```

### 4.2 Modelo `Produto` base

Conforme já analisado, o tutorial deve **começar pelo modelo em `produtos/models.py`, adaptado**:

```python
from django.db import models

class Produto(models.Model):
    nome = models.CharField(max_length=100)
    preco = models.DecimalField(max_digits=8, decimal_places=2, default=0)
```

> Modelo com `id, nome, preco`. `descricao`, `estoque`, `criado_em`, `atualizado_em` e
> `Categoria` (incluindo a `ForeignKey`) devem ficar **para depois** (evolução). O objetivo é
> espelhar o dataset `{id, nome, preco}` do README (5 → depois 60 produtos).

### 4.3 Serializer (`serializers.py`)

```python
from rest_framework.serializers import ModelSerializer
from .models import Produto

class ProdutoSerializer(ModelSerializer):
    class Meta:
        model = Produto
        fields = "__all__"

```

> No DRF a **validação** de `nome` (2–100) e `preco` (>0) pode ser reforçada com `validators` ou
> `validate_nome()`/`validate_preco()`, aproximando o comportamento manual das Aulas 7 do Express.

### 4.4 ViewSet com filtros, busca e ordenação (`views.py`)

```python
from rest_framework.viewsets import ModelViewSet
from rest_framework.filters import SearchFilter, OrderingFilter
from django_filters.rest_framework import DjangoFilterBackend

class ProdutoViewSet(ModelViewSet):
    queryset = Produto.objects.all()
    serializer_class = ProdutoSerializer
    filter_backends = [DjangoFilterBackend, SearchFilter, OrderingFilter]
    filterset_fields = {"preco": ["gte", "lte"]}
    search_fields = ["nome"]
    ordering_fields = ["nome", "preco"]
    ordering = ["-preco"]
```

### 4.5 Dependências (`requirements.txt`)

```
Django==5.2.5
djangorestframework==3.16.1
django-filter==25.1
drf-spectacular==0.28.0
```

> Para o tutorial, em vez de versionar o `db.sqlite3`, recomenda-se rodar `migrate` e,
> opcionalmente, um comando para carregar os 60 produtos de `produtos.json` (seed), em
> contraste com as aulas em memória do README. O carregamento do dataset fica **para depois**.

---

## 5. Contrato HTTP — metas de equivalência

O exercício central do tutorial DRF é traduzir o **contrato do README** para o mundo DRF, anotando
onde ele **equivale** e onde **diverge** (sem corrigir repos):

| Operação | Express / FastAPI (README) | DRF típico |
|----------|------------------------------|------------|
| GET coleção | 200 + array | `ListAPIView` → 200 + array |
| GET por ID | 200 + objeto | `RetrieveAPIView` → 200 + objeto |
| POST | 201 + criado | `CreateModelMixin` → **201** + criado |
| PUT | 200 + atualizado | `UpdateModelMixin` → 200 |
| DELETE | **204** (sem corpo) | `DestroyModelMixin` → **204** |
| Validação | `400` + `{detail: {campo: msg}}` | **`400`** (DRF) com o próprio `detail` |
| Não encontrado | 404 | **`404`** |
| Paginação | `{page, page_size, total_pages,}` | `{count, next, previous, results}` |

> **Nota didática:** o DRF usa formatos de paginação e de erro próprios (`count/next/previous`,
> `detail` via `ValidationError`). Isso é **esperado** e deve ser registrado como divergência
> mantidas, não corrigido — reforçando que "o contrato HTTP" é sobre métodos, status e semântica,
> não sobre a forma exata do corpo.

---

## 6. Pontos para desenvolvimento futuro

- Escrever as aulas efetivas, modelando o `Produto(id, nome, preco)`.
- Decidir como **carregar os 60 produtos** do `produtos.json` (comando de gerenciamento ou fixture).
- Explorar `ViewSets` (ModelViewSet) **vs** `APIView`/`GenericAPIView` — quando cada um compensa.
- Tratar paginação: manter o formato nativo DRF ou **customizar** para `page/page_size/total_pages`
  (decisão ainda não fechada).
- Incluir `Categoria`/relacionamentos/evolução do modelo apenas quando o núcleo estiver fechado.
- Comparar `/docs` (Swagger DRF) com `/docs` do FastAPI e a documentação gerada pelo FastAPI.

---

## Conclusão

> A didática da série 2–13 permanece: **o framework muda a forma de implementar; o contrato HTTP
> permanece.** O DRF confirma isso ao entregar, com poucas linhas declarativas, o mesmo CRUD,
> validação, filtros, busca, ordenação e documentação que as versões manuais.