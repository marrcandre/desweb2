# Auditoria Final do Tutorial

> Documento gerado em modo **somente leitura**. Nenhum arquivo de código, README ou configuração foi alterado durante esta auditoria (com exceção da criação deste próprio relatório). As referências a "Aula 26 — Paginação" e à sua correção já refletem uma edição feita **antes** desta auditoria, em sessão anterior — não nesta etapa.

## 1. Resumo executivo

O tutorial (`desweb2/README.md`, 2.748 linhas) constrói a mesma API de Produtos em três tecnologias — **Express**, **FastAPI** e **Django + DRF** — em 27 aulas, divididas em 7 partes. A auditoria comparou o conteúdo didático (README) com o código real dos quatro repositórios (`express-bsi4`, `fastapi-bsi4`, `django-bsi4`, e os documentos de `desweb2/docs/`).

**Nível geral de consistência:** bom, com uma lacuna estrutural já sanada (paginação no Django) e um número pequeno de problemas técnicos concretos, sendo um deles crítico.

**Principais pontos positivos**
- Express e FastAPI estão em alto grau de paridade conceitual e de contrato HTTP (mesmas rotas `/api/produtos/`, mesmo formato de erro `detail`, mesmos parâmetros `preco_minimo`/`preco_maximo`/`search`/`ordering`/`page`/`page_size`, mesmos códigos HTTP 200/201/204/400/404). Isso está documentado e confirmado no código real.
- A Parte 7 (Django) evolui de forma pedagogicamente sólida: projeto → Model → Admin → Serializer → endpoint → ViewSet/Router → validações → filtros → ordenação/busca → **paginação** (Aula 26, adicionada recentemente) → evolução do modelo (marca/estoque/descrição).
- O código real de `django-bsi4` já está bastante avançado (4 migrations, filtros, busca, ordenação e uma classe de paginação `ProdutoPagination` que bate exatamente com o contrato ensinado na Aula 26).
- `docs/plan.md` já previa explicitamente (linha 377) que "o Django poderá adotar o mesmo contrato [de paginação] futuramente, depois da conclusão das Aulas 17–27" — a Aula 26 recém-criada está alinhada a essa decisão.

**Principais problemas encontrados**
- 🔴 Um bloco de código corrompido/duplicado na Aula 21 do README (URLs do Django), que impede seguir a aula literalmente.
- 🟠 As validações customizadas ensinadas nas Aulas 23, 26 (marca/estoque/descrição) **não existem** no `produtos/serializers.py` real do `django-bsi4` — o código de referência está atrasado em relação ao que o tutorial ensina.
- 🟠 Contradição de pré-requisitos: o README pede Python 3.10+ para Django, mas `django-bsi4/pyproject.toml` exige Python 3.14+.
- 🟡 `docs/plan.md` (seção 20) descreve uma evolução do Django baseada em `Categoria` com `ForeignKey`, que nunca foi implementada nem no tutorial nem no código real (a evolução real usa `marca`/`estoque`/`descricao`, sem relacionamento).
- 🟡 `aula14.py` do FastAPI existe no disco mas não é mencionado no README do `fastapi-bsi4` nem no `desweb2/README.md`.

**A Parte Django cobre adequadamente Express e FastAPI?** Majoritariamente sim, após a correção recente de paginação. Restam pequenas lacunas (validações customizadas não implementadas no código de referência, e a divergência de versão do Python).

**Os documentos de `docs/` estão alinhados com o tutorial?** Parcialmente. `analise-estado-atual.md` é, por design, um retrato histórico e está desatualizado nos números de aula (ex.: cita `10-paginacao`/`11-persistencia-json` antigos), mas isso é reconhecido pelo próprio documento (seção 20). `plan.md` tem ao menos uma decisão (Categoria/ForeignKey) que não foi seguida na prática.

---

## 2. Matriz Express → Django

| Conceito | Express | Django | Status | Observação | Gravidade |
|---|---|---|---|---|---|
| GET coleção | `GET /api/produtos/` (README Aula 02; `express-bsi4/aula2_api_basica_get_colecao.js`) | `GET /api/produtos/` via `ProdutoViewSet`/`DefaultRouter` (README Aula 22) | ✅ Coberto | Mesmo contrato de rota. | — |
| GET por ID | `GET /api/produtos/:id/`, 404 com `{"detail": "..."}` | `GET /api/produtos/{id}/`, 404 padrão do DRF | ✅ Coberto | Nomenclatura de path param já unificada (antiga divergência `item_id`/`id_produto` citada em `analise-estado-atual.md` não existe mais no código atual). | — |
| POST (criar) | `POST /api/produtos/`, 201, objeto direto | `POST /api/produtos/`, 201 via `ModelViewSet.create` | ✅ Coberto | — | — |
| PUT (atualizar) | `PUT /api/produtos/:id/`, 200, objeto direto | `PUT /api/produtos/{id}/`, 200 via `ModelViewSet.update` | ✅ Coberto | Django também expõe `PATCH` automaticamente (o `ModelViewSet` sempre inclui `partial_update`), enquanto o plano explicitamente exclui `PATCH` da etapa inicial (`plan.md` linha ~226). Isso é uma diferença estrutural do framework, não um erro, mas **não está mencionado em nenhuma aula do Django** que o `PATCH` "aparece de graça". | 🔵 Baixo |
| DELETE | `DELETE /api/produtos/:id/`, 204 sem corpo | `DELETE /api/produtos/{id}/`, 204 via `ModelViewSet.destroy` | ✅ Coberto | — | — |
| Formato de erro (`detail`) | `{"detail": "..."}` / `{"detail": {campo: msg}}` | Erros padrão do DRF já usam a chave `detail` nativamente | ✅ Coberto | Coincidência favorável: o DRF já usa `detail` por padrão, então nenhuma configuração extra foi necessária. | — |
| Validação de `nome`/`preco` | Manual, com `trim`, 2–100 chars, preço > 0 e ≤ 2 casas decimais (`aula7_validacao.js`) | `validate_nome`/`validate_preco` ensinados na Aula 23 do README | ⚠️ Parcialmente coberto | O **tutorial** ensina os métodos `validate_nome`/`validate_preco` (README Aula 23), mas o **código real** de `produtos/serializers.py` em `django-bsi4` não contém nenhum `validate_*` — usa apenas o `ModelSerializer` padrão. Ou seja, o repositório de referência está desalinhado com o que a aula ensina. | 🟠 Importante |
| Filtros de preço (`preco_minimo`/`preco_maximo`) | Query params `preco_minimo`/`preco_maximo` (`aula8_filtros.js`) | `ProdutoFilter` com `preco_minimo`/`preco_maximo` (README Aula 24; `produtos/filters.py`) | ✅ Coberto | Nomes de parâmetro idênticos nas duas tecnologias. | — |
| Busca textual (`search`) | Query param `search`, case-insensitive (`aula9_busca.js`) | `SearchFilter` com `search_fields` (README Aula 25) | ✅ Coberto | — | — |
| Ordenação (`ordering`) | Query param `ordering`, aceita `-campo` (`aula10_ordenacao.js`) | `OrderingFilter` com `ordering_fields` (README Aula 25) | ✅ Coberto | — | — |
| Persistência | Arquivo `produtos.json` (`aula11_persistencia_json.js`) | SQLite + Django ORM (Aula 18) | 🔄 Coberto de outra forma | Diferença deliberada e documentada em `plan.md` seção 13 (o Django usa ORM para mostrar abstração). Correto por design. | — |
| Paginação (`page`/`page_size`/`total_pages`/`results`) | Implementação manual (`aula12_paginacao.js`) | `ProdutoPagination(PageNumberPagination)` customizada (README Aula 26; `produtos/pagination.py`) | ✅ Coberto (recém-adicionado) | Contrato de resposta idêntico nas duas tecnologias. Antes da última edição do README esta aula não existia (lacuna já registrada e corrigida nesta mesma sessão de trabalho). | — |
| Evolução do modelo — `marca` | README Aula 14 (Express + FastAPI) | README Aula 27, exercício "Marca"; implementado em `produtos/models.py` (`marca = CharField(..., default="Genérica")`) e em `produtos/filters.py`/`views.py` | ✅ Coberto | Campo, filtro, busca e ordenação existem no código real. Falta apenas a validação (`validate_marca`), ver linha da tabela de validação acima. | 🟠 Importante (herdado do item de validação) |
| Evolução do modelo — `estoque` | README Aula 15 | README Aula 27, exercício "Estoque"; `estoque = IntegerField(default=0)`, `estoque_minimo`/`estoque_maximo` em `ProdutoFilter` | ✅ Coberto | Mesma observação: falta `validate_estoque` no serializer real. | 🟠 Importante (herdado) |
| Evolução do modelo — `descricao` | README Aula 16 | README Aula 27, exercício "Descrição"; `descricao = TextField(blank=True, null=True)`, incluída em `search_fields`/`ordering_fields` | ✅ Coberto | Mesma observação: falta `validate_descricao` no serializer real. | 🟠 Importante (herdado) |
| Documentação automática | Swagger/`/docs`, `/redoc` (FastAPI apenas; Express não tem) | `/api/schema/`, `/api/docs/`, `/api/redoc/` via drf-spectacular (README Aula 21; `config/urls.py`) | 🔄 Coberto de outra forma | Express nunca teve doc automática (não é uma lacuna: não é um recurso nativo do Express). | — |
| Testes HTTP versionados (Bruno) | Coleção completa em `express-bsi4/http/express/` (13 pastas de aula, 44 requisições) | Não existe pasta `http/django/`; o README aponta Swagger/Admin como interface de teste do Django | ⚠️ Parcialmente coberto | `plan.md` seção 16.4 diz que as três tecnologias devem, "sempre que tecnicamente possível", ser submetidas ao mesmo conjunto de testes HTTP versionados via Bruno. A tabela de visão geral do README (seção 5) já reflete a decisão real (Django usa Swagger/Admin, não Bruno), mas isso contradiz o texto de `plan.md`. Ver seção 6.3 deste relatório. | 🟡 Moderado |

---

## 3. Matriz FastAPI → Django

| Conceito | FastAPI | Django | Status | Observação | Gravidade |
|---|---|---|---|---|---|
| GET coleção | `GET /api/produtos/`, array direto (`aula2_api_basica_get_colecao.py`) | `GET /api/produtos/`, paginado por padrão (`ProdutoViewSet`) | 🔄 Coberto de outra forma | Diferença esperada: a partir da Aula 26, o Django sempre devolve o envelope paginado, enquanto o FastAPI só pagina a partir da Aula 12/13. Isso é coerente com o contrato fechado (paginação é a política padrão da listagem). | — |
| GET por ID | `GET /api/produtos/{id}/`, 404 com `detail` | `GET /api/produtos/{id}/`, 404 padrão do DRF | ✅ Coberto | — | — |
| POST/PUT/DELETE | 201/200/204, `HTTPException(detail=...)` | 201/200/204, serializer + `ModelViewSet` | ✅ Coberto | — | — |
| Validação (Pydantic `Field`) | `ProdutoInput` com regras manuais dentro de `validar_produto()` (não usa `Field(min_length=...)` nativamente — ver observação) | `validate_nome`/`validate_preco` ensinados na Aula 23 | ⚠️ Parcialmente coberto | Mesma lacuna já registrada na matriz anterior: o serializer real do Django não tem `validate_*`. Adicionalmente, vale nota lateral: nas aulas de FastAPI, `ProdutoInput` declara os campos como opcionais (`nome: str \| None = None`) e a obrigatoriedade é reforçada manualmente em `validar_produto()` — um mecanismo hídrido que poderia ser mencionado explicitamente como "Pydantic puro vs. validação manual adicional" para reforçar a comparação pedagógica pretendida. | 🟠 Importante (validação Django) / 🔵 Baixo (nota pedagógica FastAPI) |
| Filtros | `preco_minimo`/`preco_maximo` como `str` convertidos manualmente (`aula8_filtros.py`) | `ProdutoFilter` declarativo (`django_filters`) | 🔄 Coberto de outra forma | Nomes de parâmetro idênticos; mecanismo é naturalmente diferente (`django-filter` vs. filtragem manual de lista). | — |
| Busca | `search` case-insensitive manual | `SearchFilter` | 🔄 Coberto de outra forma | — | — |
| Ordenação | `ordering` manual com `sort(key=..., reverse=...)` | `OrderingFilter` | 🔄 Coberto de outra forma | — | — |
| Persistência JSON | `produtos.json`, funções `carregar_produtos()`/`salvar_produtos()` (Aula 11) | SQLite + ORM | 🔄 Coberto de outra forma | Esperado — ver `plan.md` seção 13. | — |
| Paginação | Modelo Pydantic `RespostaPaginada` com `{page, page_size, total_pages, results}` (Aula 12) | `ProdutoPagination` com o mesmo formato de saída (Aula 26) | ✅ Coberto | Contrato idêntico confirmado nos dois códigos reais. | — |
| Documentação automática | `/docs`, `/redoc` nativos do FastAPI | `/api/schema/`, `/api/docs/`, `/api/redoc/` via drf-spectacular | ✅ Coberto | — | — |
| `aula14.py` (exercício "adicionar marca") | Existe no disco (`fastapi-bsi4/aula14.py`), usa `produtos_aula14.json`, mas **não implementa o campo `marca`** e **não está listado no README** de `fastapi-bsi4` nem citado em `desweb2/README.md` | Equivalente Django (Aula 27, exercício "Marca") está completo no código (`marca` no `models.py`, `filters.py`, `views.py`) | ⚠️ Parcialmente coberto | Ver detalhes na seção 5 (Erros técnicos, item E3). Pode ser um arquivo de apoio esquecido ou um "ponto de partida" para o aluno fazer o exercício — não há evidência textual de qual é a intenção. Necessita revisão manual. | 🟡 Moderado |

---

## 4. Consistência pedagógica

### Padrão consistente
- Nomenclatura de query params idêntica entre Express e FastAPI (`preco_minimo`, `preco_maximo`, `search`, `ordering`, `page`, `page_size`) e replicada no Django via `ProdutoFilter`/`OrderingFilter`/`SearchFilter`/`ProdutoPagination`.
- Sequência conceitual igual nas duas primeiras tecnologias e coerente na terceira: GET → GET por ID → POST → PUT → DELETE → validação → filtros → busca → ordenação → paginação, na mesma ordem relativa (Aulas 17–26 do Django seguem essa progressão, apenas trocando "busca" e "ordenação" de posição, que aparecem juntas na Aula 25 — diferença aceitável, pois no DRF os dois filtros costumam ser configurados juntos).
- Contrato de erro `{"detail": ...}` idêntico nas três tecnologias.
- Convenção `Produto(id, nome, preco)` como base comum, respeitada nas três tecnologias antes da evolução (marca/estoque/descrição).

### Diferenças justificadas (não são problemas)
- Persistência: memória → JSON (Express/FastAPI) vs. SQLite/ORM (Django) — decisão pedagógica explícita.
- `PATCH` disponível "de graça" no Django (via `ModelViewSet`) mas fora de escopo no Express/FastAPI — decorre da abstração do framework, não de uma escolha de conteúdo.
- Documentação automática nativa apenas em FastAPI e Django (Express nunca teve, por não ser um recurso do framework).

### Inconsistências que merecem correção
- 🟠 As validações customizadas ensinadas nas Aulas 23/26/27 do Django não estão implementadas no `serializers.py` real de `django-bsi4` — quem usar esse repositório como gabarito encontrará uma API sem as validações que o tutorial promete.
- 🟡 A pasta `http/django/` (coleção Bruno) não existe, enquanto `plan.md` (seção 16.4) afirma que as três tecnologias devem compartilhar o mesmo conjunto de testes HTTP "sempre que tecnicamente possível". Se a decisão de usar apenas Swagger para o Django for definitiva, `plan.md` deveria ser atualizado para não sugerir o contrário.
- 🟡 `aula14.py` do FastAPI é um artefato órfão (não documentado, não referenciado, incompleto frente ao próprio nome do arquivo).

---

## 5. Erros técnicos encontrados

### E1 — 🔴 CRÍTICO — Bloco de código corrompido/duplicado na Aula 21 (Django)
- **Arquivo/seção:** `desweb2/README.md`, Aula 21 — "Primeiro endpoint e Swagger/OpenAPI", subseção "3. Criando as URLs" (por volta da linha 2244–2270).
- **Problema:** o bloco de código Python mostrado para `config/urls.py` está corrompido: começa com o conteúdo correto da Aula 21 (`ProdutoListAPIView`), mas a lista `urlpatterns` nunca é fechada — na sequência aparecem, sem nova cerca de código, um segundo bloco de imports duplicados e um segundo `urlpatterns = [...]` que já usa `ProdutoViewSet` e `DefaultRouter`, conceitos que só são introduzidos na Aula 22 seguinte.
- **Evidência (trecho literal):**
  ```python
  urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/produtos/", ProdutoListAPIView.as_view(), name="produto-list"),
    path("api/schema/", SpectacularAPIView.as_view(), name="schema"),
    path("api/docs/", SpectacularSwaggerView.as_view(url_name="schema"), name="swagger-ui"),
  from django.contrib import admin
  from django.urls import include, path
  from drf_spectacular.views import (
      SpectacularAPIView,
      SpectacularRedocView,
      SpectacularSwaggerView,
  )
  from rest_framework.routers import DefaultRouter

  from produtos.views import ProdutoViewSet

  router = DefaultRouter()
  router.register("produtos", ProdutoViewSet, basename="produto")

  urlpatterns = [
  ```
- **Impacto:** um aluno que copiar o bloco literalmente da Aula 21 terá um arquivo `urls.py` com erro de sintaxe (colchete não fechado, imports duplicados, `ProdutoViewSet` inexistente nesse ponto do tutorial). Impede seguir a aula como descrito.
- **Gravidade:** 🔴 Crítico.
- **Sugestão de correção futura (não aplicada agora):** restaurar o bloco da Aula 21 para conter apenas a versão com `ProdutoListAPIView` e fechar a lista corretamente; o bloco duplicado da Aula 22 já existe corretamente mais abaixo no arquivo e pode ser removido daqui.

### E2 — 🟠 IMPORTANTE — Validações do Django ensinadas no README, ausentes no código de referência
- **Arquivo/seção:** `desweb2/README.md` Aulas 23, 27; código real `django-bsi4/produtos/serializers.py`.
- **Problema:** o README ensina `validate_nome`, `validate_preco` (Aula 23) e depois `validate_marca`, `validate_estoque`, `validate_descricao` (Aula 27, dentro dos blocos `<details>`). O arquivo real `produtos/serializers.py` contém apenas:
  ```python
  class ProdutoSerializer(serializers.ModelSerializer):
    class Meta:
      model = Produto
      fields = ("id", "nome", "preco", "marca", "estoque", "descricao")
  ```
  sem nenhum método `validate_*`.
- **Impacto:** o repositório `django-bsi4`, se usado como gabarito/solução de referência, não corresponde ao que o tutorial promete; um aluno que comparar sua implementação com o repositório de referência não vai encontrar as validações.
- **Gravidade:** 🟠 Importante.
- **Sugestão:** implementar os métodos `validate_*` em `produtos/serializers.py` conforme os blocos já escritos no README, ou deixar claro em algum lugar que `django-bsi4` representa um estado "sem os exercícios de validação resolvidos".

### E3 — 🟡 MODERADO — `fastapi-bsi4/aula14.py` não documentado e incompleto
- **Arquivo/seção:** `fastapi-bsi4/aula14.py`; `fastapi-bsi4/README.md` (não o menciona); `desweb2/README.md` Aula 14 (não referencia o arquivo).
- **Problema:** o arquivo existe no disco, usa um arquivo de dados próprio (`produtos_aula14.json`, diferente do `produtos.json` usado pelas Aulas 11–13), mas seu `ProdutoInput` não contém o campo `marca` — apesar de comentários internos indicarem "Aula 14 - Exercício 1 - Adicionar Marca no Produto".
- **Impacto:** confunde o aluno que navegar pelos arquivos do repositório e encontrar `aula14.py` sem saber se deveria completá-lo ou se é um resquício.
- **Gravidade:** 🟡 Moderado. Necessita revisão manual para decidir se é ponto de partida do exercício ou resíduo esquecido.

### E4 — 🟠 IMPORTANTE — Requisito de versão do Python contraditório para Django
- **Arquivo/seção:** `desweb2/README.md`, seção "4. Pré-requisitos" (linha ~44): *"Python (versão 3.10 ou superior) — para o FastAPI e Django"*; vs. `django-bsi4/pyproject.toml`: `requires-python = ">=3.14"` e `django-bsi4/.python-version`: `3.14`.
- **Impacto:** um aluno com Python 3.10–3.13 instalado (a versão anunciada como suficiente) não conseguirá rodar `uv sync`/`uv add` no projeto Django, pois o `pyproject.toml` exige 3.14+.
- **Gravidade:** 🟠 Importante — pode travar a Parte 7 inteira para quem seguir literalmente o pré-requisito informado.
- **Necessita revisão manual:** não é possível determinar, sem acesso ao ambiente do autor, se o pin em `>=3.14` foi intencional (ex.: usar recursos novos da linguagem) ou um artefato padrão gerado pelo `uv init` na máquina do autor (que pode ter tido Python 3.14 já instalado como versão default).

### E5 — 🔵 BAIXO — `django-bsi4/README.md` vazio
- **Arquivo/seção:** `django-bsi4/README.md`.
- **Problema:** o arquivo existe mas não tem conteúdo, diferente de `express-bsi4/README.md` e `fastapi-bsi4/README.md`, que têm instruções de instalação/execução completas.
- **Impacto:** baixo, pois o `desweb2/README.md` já documenta os comandos de setup do Django nas Aulas 17+. Mas cria uma assimetria entre os três repositórios.
- **Gravidade:** 🔵 Baixo.

### E6 — 🔵 BAIXO — Pasta `src/` não utilizada em `django-bsi4`
- **Arquivo/seção:** `django-bsi4/src/django_bsi4/__init__.py` (vazio).
- **Problema:** resíduo do scaffold gerado por `uv init --app .` (Aula 17) — a aplicação Django real vive em `config/`/`produtos/` na raiz, não em `src/`.
- **Impacto:** nenhum funcional; pode confundir quem explorar a árvore de diretórios.
- **Gravidade:** 🔵 Baixo.

---

## 6. Comparação com `docs/`

### 6.1 Conteúdo de `docs/` que está faltando no tutorial
- `plan.md` (seção 20) planeja uma evolução do Django com entidade `Categoria` e relacionamento `ForeignKey`. O `desweb2/README.md` (Aula 27) evolui o modelo apenas com campos escalares (`marca`, `estoque`, `descricao`), sem introduzir `Categoria` nem relacionamentos. **Necessita decisão do autor:** ou `plan.md` está desatualizado (a decisão mudou), ou falta uma aula futura de "relacionamentos" no README.
- `plan.md` (seção 16.4–16.5) descreve uma estratégia de testes HTTP compartilhados via Bruno para as três tecnologias, incluindo Django. O README não tem nenhuma coleção Bruno para Django (usa apenas Swagger/Admin). Isso não está refletido/atualizado em `plan.md`.

### 6.2 Conteúdo do tutorial que está faltando em `docs/`
- A Aula 26 (Paginação no Django, recém-criada) e sua decisão de implementação (classe em `produtos/pagination.py` + configuração global em `settings.py`) não estão registradas em nenhum documento de `docs/`. `analise-estado-atual.md` (seção 10) ainda afirma "Paginação: NÃO configurada" no Django — o que já não é verdade, tanto no README quanto no código real.
- A renumeração de "Aula 26 — Evoluindo o Produto"/"Aula 27 — Exercício" para "Aula 26 — Paginação"/"Aula 27 — Evoluindo o Produto" (feita nesta mesma linha do tempo de edições) não está registrada em `docs/`.

### 6.3 Contradições
- **Testes via Bruno:** `plan.md` (seção 16.4) diz que as três tecnologias devem ser testadas com o mesmo conjunto de requisições Bruno; a tabela de visão geral do `README.md` (seção 5) já mostra Django usando apenas "Swagger `/api/docs/` e Django Admin `/admin/`", sem Bruno. Uma dessas duas fontes está desatualizada em relação à decisão real.
- **Categoria/ForeignKey:** `plan.md` (seção 20) planeja isso como evolução natural do Django; o tutorial real não usa `Categoria` em nenhum momento.
- **Estado da paginação Django:** `analise-estado-atual.md` (seção 10 e itens 9/13 da lista de lacunas) descreve a paginação do Django como pendente/futura; o README atual já a implementa na Aula 26, e o código real (`produtos/pagination.py`) já corresponde a isso.

### 6.4 Documentação possivelmente obsoleta
- `analise-estado-atual.md` é, pelo próprio texto (nota no topo do arquivo: *"Registro histórico... A documentação vigente da Parte 7 está no README e começa na Aula 17"*), assumido como histórico. Ainda assim, seções específicas (10, 13, 14 item 9/13) descrevem um estado de paginação/validação do Django que já mudou, sem uma seção equivalente à 20 (que já existe para o Express) documentando a atualização para o Django. **Sugestão:** um "seção 21" análoga à seção 20 (que já documenta a refatoração do Express) poderia registrar o estado atual do Django (paginação implementada, evolução marca/estoque/descrição concluída no código, validações ainda pendentes).
- `plan.md` seção 20 (evolução via Categoria) parece obsoleta frente à direção real tomada pelo tutorial (evolução via campos escalares).

---

## 7. Problemas de nomenclatura e padronização

| Item | Observação |
|---|---|
| Nome de path param | Já unificado como `:id` (Express) / `{id}` (FastAPI/Django) no código atual — a divergência antiga (`item_id`/`id_produto`) citada em `analise-estado-atual.md` não existe mais; documento histórico desatualizado neste ponto específico. |
| Arquivo de dados FastAPI Aula 14 | `produtos_aula14.json` quebra o padrão `produtos.json` usado pelas Aulas 11–13; não há explicação no README para essa mudança de nome. |
| `django-bsi4/src/` | Pasta remanescente do scaffold `uv init --app .`, sem uso real — nome pode sugerir (incorretamente) que o código-fonte da aplicação está lá. |
| Termo "Exercício" na tabela de aulas | Corrigido nesta linha do tempo: a tabela de visão geral (README seção 7) tinha duas linhas ("26 — Evoluindo o Produto" e "27 — Exercício") mapeando para um único cabeçalho `## 📘 Aula 26` no corpo do texto; já ajustado para "26 — Paginação" / "27 — Evoluindo o Produto" nesta mesma sequência de edições. |

---

## 8. Lacunas de conteúdo

Lista do que foi ensinado em Express/FastAPI e o estado correspondente no Django, em ordem de prioridade pedagógica:

1. ~~**Paginação**~~ — **já resolvida** (Aula 26, adicionada nesta mesma linha do tempo de edições, com código real já compatível em `produtos/pagination.py`).
2. **Validação customizada por campo** (`validate_nome`, `validate_preco`, `validate_marca`, `validate_estoque`, `validate_descricao`) — ensinada no README (Aulas 23 e 27), mas **ausente no código real** de `django-bsi4/produtos/serializers.py`. Prioridade alta para fechar a paridade entre tutorial e repositório de referência.
3. **Coleção de testes HTTP versionada para Django** — Express e FastAPI têm pastas `http/express/` e `http/fastapi/` completas (13 pastas de aula, dezenas de requisições `.bru`); Django não tem equivalente. Se a decisão for manter Django só no Swagger, isso deveria estar explícito em `plan.md` (hoje contradiz a seção 16.4).
4. **Evolução com relacionamento (`Categoria`/`ForeignKey`)** — prevista em `plan.md`, nunca implementada. Pode ser uma decisão deliberada de simplificação (não necessariamente uma lacuna), mas precisa de uma atualização explícita em `plan.md` para deixar de ser uma contradição.

---

## 9. Recomendações de correção

Ordenadas por prioridade, sem implementação nesta etapa:

### 🔴 Críticas
1. Corrigir o bloco de código corrompido da Aula 21 (`config/urls.py`) no `desweb2/README.md` — remover a duplicação/mistura com o conteúdo da Aula 22 (ver E1).

### 🟠 Importantes
2. Implementar `validate_nome`, `validate_preco`, `validate_marca`, `validate_estoque`, `validate_descricao` em `django-bsi4/produtos/serializers.py`, alinhando o código real ao que o README ensina (ver E2).
3. Resolver a contradição de versão do Python para o Django: ajustar `desweb2/README.md` (pré-requisitos) para citar a versão mínima realmente exigida por `django-bsi4/pyproject.toml`, ou rebaixar o `requires-python` do projeto para bater com "3.10 ou superior" anunciado (ver E4).

### 🟡 Moderadas
4. Decidir e documentar de forma consistente se o Django deve ou não ter uma coleção Bruno equivalente às de Express/FastAPI; atualizar `plan.md` (seção 16.4) ou o README para refletir a decisão final (ver seção 6.3).
5. Esclarecer o papel de `fastapi-bsi4/aula14.py` — documentá-lo no README do `fastapi-bsi4` e no `desweb2/README.md`, ou removê-lo/renomeá-lo se for resíduo (ver E3).
6. Atualizar `docs/plan.md` seção 20 para refletir a decisão real de evolução do Django (campos escalares em vez de `Categoria`/`ForeignKey`), ou implementar `Categoria` caso a decisão ainda esteja de pé.
7. Registrar em `docs/analise-estado-atual.md` uma seção equivalente à "20 — Atualização" (já existente para o Express) documentando o estado atual do Django (paginação implementada, evolução marca/estoque/descrição concluída, validações pendentes).

### 🔵 Baixas
8. Preencher `django-bsi4/README.md` com instruções básicas de setup, para paridade com `express-bsi4`/`fastapi-bsi4`.
9. Remover ou documentar a pasta `django-bsi4/src/` (resíduo do scaffold `uv init --app .`).

---

## 10. Checklist final

| Item | Situação |
|---|---|
| Express completamente coberto por Django | ⚠️ (falta apenas a implementação real das validações customizadas) |
| FastAPI completamente coberto por Django | ⚠️ (idem, mais a nota sobre `aula14.py` órfão) |
| Padrão pedagógico consistente | ⚠️ (pequenas assimetrias: Bruno ausente no Django, `PATCH` "de graça" não mencionado) |
| README tecnicamente consistente | ❌ (bloco de código corrompido na Aula 21; pré-requisito de Python contraditório) |
| `docs/` alinhado ao tutorial | ⚠️ (plan.md com decisão de Categoria não implementada; analise-estado-atual.md desatualizado quanto à paginação/validação do Django) |
| Existem lacunas importantes | Sim (validações customizadas do Django não implementadas no código real) |
| Existem erros técnicos importantes | Sim (1 crítico — Aula 21; 1 importante — versão do Python) |
| Existem documentos potencialmente obsoletos | Sim (`analise-estado-atual.md` seções 10/13/14 sobre paginação Django; `plan.md` seção 20 sobre Categoria) |
