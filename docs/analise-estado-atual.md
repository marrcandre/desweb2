# Relatório técnico — Estado atual do repositório Desenvolvimento Web 2

## 0. Amostra analisada

Foram lidos integralmente: `desweb2/docs/plan.md`, `desweb2/README.md`, os READMEs de `express-bsi4`, `fastapi-bsi4` e `django-bsi4`, todos os arquivos `.js` de `express-bsi4`, todos os `.py` de `fastapi-bsi4`, todos os arquivos relevantes de `django-bsi4` (models, serializers, views, filters, admin, urls, settings, migration, requirements), o `bsi4.code-workspace`, os `.gitignore` e os dados de apoio (`produtos.json`).

---

## 1. Estrutura do repositório

```
/home/marco/projetos/bsi4/
├── bsi4.code-workspace        # VS Code workspace (agrupa os 5 projetos)
├── desweb2/                   # (único repositório git) conteúdo didático
│   ├── docs/plan.md           # planejamento pedagógico (referência)
│   ├── README.md              # conteúdo de aulas (Aulas 1–14)
│   ├── LICENSE
│   └── .vscode/
├── express-bsi4/              # aplicação Express (progressivos)
├── fastapi-bsi4/              # aplicação FastAPI (progressivos)
├── django-bsi4/               # aplicação Django + DRF
└── vuejs-bsi4/                # VAZIO (diretório criado, sem conteúdo)
```

Observações relevantes:

- **Vue** não existe ainda — apenas um placeholder `vuejs-bsi4/` vazio. Coerente com o planejamento (Vue só depois da consolidação do backend).
- **Express e FastAPI** possuem arquivos progressivos nomeados por aula (`aula2a` … `aula6`). Nenhum usa a convenção `NN-descricao` nem `NN-api-completa` do plan.
- **Django** é uma aplicação única (não progressiva) com `Categoria` e `Produto`.
- Cada subprojeto tem **README próprio**, o que **diverge** da decisão "um único README" do plano.
- Existem arquivos de **dados de apoio** (`fastapi-bsi4/produtos.json` e `django-bsi4/produtos.json`), com finalidades distintas (dados carregados por FastAPI vs. fixture de dump do Django).

---

## 2. Análise do Express

Rota padrão em todos os arquivos: **`/produtos`** (sem `/api/` e sem barra final). Portas variam entre **3000 e 8000**.

| Arquivo | Finalidade | Conceitos | Endpoints | Modelo | Persist. | CRUD | Validação | Filtros | Busca | Ordenação | Paginação | Doc | Nível didático |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `aula2a.js` | GET lista fixa + POST eco | GET, POST, JSON, rota | `GET /produtos`, `POST /produtos` | id/nome/preco (hardcoded) | memória (const literal) | POST parcial (eco) | não | não | não | não | não | não | aula 2 — API-base |
| `aula2b.js` | lista em memória + POST real com id | GET, POST, geração de id | `GET/POST /produtos` | id/nome/preco | memória (array `let`) | POST | não | não | não | não | não | não | aula 2 — API-base |
| `aula3a.js` | GET por id (path param) | path params, filtro fictício, 404 | `GET /produtos`, `GET /produtos/:id` | id/nome/preco | memória | só leitura | não | não (comentário fictício) | não | não | não | não | aula 3 — parâmetros |
| `aula3b_filtros.js` | listagem com filtros | query params `min_preco`/`max_preco` | `GET /produtos` | id/nome/preco | memória | só leitura | não | **sim** (preço) | não | não | não | não | aula 3 — filtros |
| `aula4_crud_completo.js` | CRUD completo | CRUD, 404, rotas dinâmicas | `GET,POST /produtos`; `GET,PUT,DELETE /produtos/:id` | id/nome/preco | memória | **completo** | **não** | não | não | não | não | não | aula 4 — CRUD |
| `aula5_…_validacao_filtro_ordenacao_paginacao.js` | CRUD avançado | validação manual, filtros, ordenação, paginação | CRUD completo (`/produtos`) | id/nome/preco | memória | completo | **sim (básica)** | sim (`min_preco`/`max_preco`) | **não** | sim (`ordenar_por`/`ordem`) | sim (`pagina`/`por_pagina`) | não | aula 5 — avançado |

**Conclusões Express:**
- **Ponto alto:** `aula5` é o exemplo mais completo (CRUD + validação + filtro + ordenação + paginação) e corresponde à etapa "API completa em memória".
- **Lacunas:** não existe **busca (`search`)**; não existe **persistência em JSON** (FastAPI já tem `aula6`; Express não tem equivalente).
- **Validação incompleta** frente ao contrato: não aplica `trim`, nem limite 2–100 no nome, nem o teto de 2 casas decimais no preço.
- **Formato de resposta inconsistente:** GET retorna array direto ou `{produtos: …}`; POST/PUT/DELETE retornam envelope `{message, dados}`/`{mensagem, dados}`. Não segue "JSON direto" do plano.

---

## 3. Análise do FastAPI

Rota padrão: **`/produtos`** (sem `/api/`, sem barra final). Documentação automática em `/docs` e `/redoc`.

| Arquivo | Finalidade | Conceitos | Endpoints | Modelo | Persist. | CRUD | Validação | Filtros | Busca | Ordenação | Paginação | Doc | Nível |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `aula2a.py` | GET lista fixa + POST eco | GET, POST, pydantic básico | `GET/POST /produtos` | id/nome/preco | memória | POST (eco) | apenas tipos Pydantic | não | não | não | não | automática | aula 2 |
| `aula2b.py` | lista + POST real, modelos in/out | modelos Pydantic, resp. tipada | `GET/POST /produtos` | id/nome/preco (`Item`) | memória | POST | Pydantic (tipos) | não | não | não | não | automática | aula 2 |
| `aula3a.py` | GET por id | path param, query fictício | `GET /produtos/{id_produto}`, `GET /produtos` | id/nome/preco | memória | só leitura | tipos | não (fictício) | não | não | não | automática | aula 3 |
| `aula3b_filtros.py` | listagem com filtros | query params, `Query` | `GET /produtos` | id/nome/preco | memória | só leitura | não | **sim** (`min_preco`/`max_preco`) | não | não | não | automática | aula 3 |
| `aula4_crud_completo.py` | CRUD completo | CRUD, `HTTPException` 404 | `GET,POST /produtos`; `GET,PUT,DELETE /produtos/{item_id}` | id/nome/preco | memória | completo | não | não | não | não | não | automática | aula 4 |
| `aula5_…_validacao_filtro_ordenacao_paginacao.py` | CRUD avançado | validação Pydantic (`Field`), filtros, ordenação, paginação | CRUD completo | id/nome/preco | memória | completo | **sim (Pydantic)** | sim | **não** | sim | sim | automática | aula 5 — avançado |
| `aula6_json_persistente.py` | CRUD + persistência | arquivo JSON (carregar/salvar) | CRUD completo | id/nome/preco | **arquivo JSON** | completo | sim | sim | **não** | sim | sim | automática | aula 6 — persistência |

**Conclusões FastAPI:**
- O `aula6` é o **único arquivo com persistência** em todas as três tecnologias progressivas — mais avançado que o Express.
- **Busca (`search`) também ausente** no FastAPI (mesma lacuna do Express).
- `aula5` e `aula6` são praticamente idênticos exceto pela persistência — talvez redundantes do ponto de vista pedagógico (validação/filtros/ordenação/paginação já estavam em `aula5`; `aula6` apenas adiciona JSON).
- Formato de sucesso usa envelope `{message, dados}` (diverge do plano); método `criar_produto` retorna **200** (default), não **201**.

---

## 4. Mapa de correspondência Express × FastAPI

| Conceito | Express | FastAPI | Situação | Observação |
|---|---|---|---|---|
| API básica (GET lista) | `aula2a.js`, `aula2b.js` | `aula2a.py`, `aula2b.py` | **alinhado** | doen paralelos, mesmo conteúdo |
| GET por ID | `aula3a.js` | `aula3a.py` | **divergente** | Express retorna `404`; FastAPI retorna `200` com `{"erro":…}` |
| Filtros (preço) | `aula3b.js`, `aula5` | `aula3b.py`, `aula5` | **alinhado** | mesmos params `min_preco`/`max_preco` |
| CRUD completo | `aula4_crud_completo.js` | `aula4_crud_completo.py` | **alinhado** | mesmo comportamento |
| Validação | `aula5` (manual) | `aula5` (Pydantic) | **alinhado em conceito, diferente em mecanismo** | esperado pelo plano (comparação manual x Pydantic) |
| Ordenação | `aula5` | `aula5` | **alinhado** | ambos `ordenar_por`/`ordem` |
| Paginação | `aula5` | `aula5` | **divergente do contrato fechado** | Hoje usam `pagina`/`por_pagina` e com **semântica diferente** (Express: offset; FastAPI: página 0-based). O contrato fechado no plan é `page`/`page_size`/`total_pages`/`results` (ver seções 8‑6 e 17) |
| Busca (`search`) | **—** | **—** | **ausente em ambos** | lacuna |
| Persistência JSON | **—** | `aula6_json_persistente.py` | **FastAPI adiantado** | Express não tem equivalente |

### Etapas existentes apenas no Express
Nenhuma exclusivamente no Express.

### Etapas existentes apenas no FastAPI
- **Persistência JSON** (`aula6`).

### Etapas com ordem diferente / precisando de divisão
- A `aula5` mistura vários conceitos (validação **+** filtro **+** ordenação **+** paginação). O plano prevê cada conceito em etapa própria (`07-validacao`, `08-filtros`, `09-busca`, `10-ordenacao`, `11-paginacao`). As `aula5`/`aula6` precisam ser **divididas por conceito**.

### Etapas que precisam ser criadas
- **Busca (`search`)** — nas duas tecnologias.
- **Persistência JSON no Express** (correspondência de `aula6`).

### Etapas que NÃO devem ser levadas para a outra tecnologia
- Nenhuma do ponto de vista de conceito. A **diferença de mecanismo** (validação manual vs. Pydantic) deve ser preservada por ser o objetivo pedagógico da comparação.

---

## 5. Sequência didática atual (inferida do conteúdo)

Os arquivos são numerados por **"aula"** (nomenclatura `aulaN`), mas essa numeração não reflete uma linearidade conceito-a-conceito:

- **Aula 2** → GET lista / POST (base) — `aula2a`, `aula2b`
- **Aula 3** → GET por ID e filtros — `aula3a`, `aula3b`
- **Aula 4** → CRUD completo
- **Aula 5** → CRUD + validação + filtro + ordenação + paginação (Express e FastAPI)
- **Aula 6** → persistência JSON (FastAPI apenas)

**Comparação com a sequência desejada:**

| Sequência desejada (plan) | Sequência atual | Divergência |
|---|---|---|
| Conceito → Express → FastAPI → Comparação → próximo | O Express foi todo feito por "aulas" e o FastAPI **espelha** as mesmas aulas | A ideia de "cada conceito primeiro Express, depois FastAPI" está **parcialmente presente** (arquivos emparelhados 2a/2b/…), mas a granularidade é por **aula**, não por **conceito** |
| Validação como etapa isolada | Mergida dentro de `aula5` | Precisa ser isolada |
| Filtros/busca/ordenação/paginação em etapas separadas | Filtros em `aula3b` + `aula5`; busca inexistente; ordenação+paginação só em `aula5` | **Filtros aparecem duas vezes**; **busca ausente**; ordenação/paginação só no fim |
| Persistência nas duas | Persistência só no FastAPI (`aula6`) | **Divergência — Express ficou para trás** |
| Arquivo final `NN-api-completa` | Não existe em nenhuma das duas | A ser criado |

**Divergências centrais:** (1) granularidade por "aula" e não por conceito; (2) conceitos aglomerados em `aula5`; (3) busca ausente; (4) persistência só no FastAPI; (5) sem arquivo final `NN-api-completa`.

---

## 6. O que deve ser preservado

| Item | Classificação |
|---|---|
| **Arquivos progressivos Express que refletem a evolução real** (`aula2a…aula5`) | **Preservar** — são a base didática; serão reordenados/renomeados, não reescritos |
| **Arquivos progressivos FastAPI** (`aula2a…aula6`) | **Preservar** — incluindo `aula6` (seu conteúdo é o modelo para a persistência do Express) |
| Emparelhamento Express/FastAPI por aula (2a, 2b, 3b, 4, 5) | **Preservar** — já segue o espírito "Express→FastAPI" |
| Validação manual no Express (`aula5`) vs Pydantic no FastAPI (`aula5`) | **Preservar** — é exatamente a comparação pedagógica desejada |
| Modelo `Produto(id, nome, preco)` | **Preservar** — é o contrato definido |
| Filtros `min_preco`/`max_preco` (nome dos parâmetros) | **Preservar** — já coincidem com o plano |
| **Documentação automática do FastAPI** (`/docs`, `/redoc`) | **Preservar** |
| **Estrutura/arquivos do Django** (Categoria, model avançado, filters, viewsets, routers, drf-spectacular) | **Preservar** — é a "evolução" intencional do plano, não deve ser reduzida |
| Dados de apoio `produtos.json` (FastAPI — dataset rico) | **Preservar** — útil para paginação/demonstração |
| `desweb2/README.md` (conteúdo das aulas) | **Preservar com pequenos ajustes** — deve se tornar "um único README com capítulos numerados", mantendo conteúdo |
| `desweb2/docs/plan.md` | **Preservar** (é a referência) |
| `plan.md` convenções (`NN`, `NN-api-completa`, contrato, códigos, `detail`) | **Preservar** — é o contrato-alvo a ser seguido |
| READMEs por subprojeto | **Substituir/reorganizar** — o plano pede README único; os READMEs por projeto divergem dessa decisão |
| `bsi4.code-workspace` | **Preservar** — útil para abrir tudo junto |
| Banco/estado local Django (`db.sqlite3`, `produtos.json` dump) | **Preservar** (dado de execução) |

---

## 7. Contrato atual das APIs (verificação)

**Modelo `Produto(id, nome, preco)` — OK**, coerente em Express e FastAPI (todos os exemplos).

**Rotas — DIVERGENTES.** Nenhum arquivo usa `/api/produtos/` (com prefixo `/api` e barra final). O padrão atual é `/produtos` e `/produtos/:id`.

| Requisito (contrato) | Express atual | FastAPI atual | Divergência |
|---|---|---|---|
| `GET /api/produtos/` | `GET /produtos` | `GET /produtos` | sem `/api/`, sem barra final |
| `GET /api/produtos/{id}/` | `GET /produtos/:id` | `GET /produtos/{item_id}` | nome do param difere (`:id` vs `{item_id}`) |
| `POST /api/produtos/` | `POST /produtos` | `POST /produtos` | sem `/api/` |
| `PUT /api/produtos/{id}/` | `PUT /produtos/:id` | `PUT /produtos/{item_id}` | idem |
| `DELETE /api/produtos/{id}/` | `DELETE /produtos/:id` | `DELETE /produtos/{item_id}` | idem |

**Demais pontos:**
- **Nomes de parâmetros:** divergentes/inconsistentes entre Express (`id`) e FastAPI (`item_id`, `id_produto`).
- **Filtros:** `min_preco`/`max_preco` — **alinhados** ao plano. ✔
- **Busca:** inexistente em ambos. ✘
- **Ordenação:** `ordenar_por`/`ordem` **divergem** do padrão do plano (`ordering=nome`, `ordering=-preco`). ✘
- **Paginação:** `pagina`/`por_pagina` **divergem** do contrato comum **já fechado** no plan (`page`, `page_size`, `total_pages`, `results`); além disso Express e FastAPI **interpretam `pagina` de forma diferente** (offset vs. página 0-based). ✘✘ (detalhe completo na seção 8‑6)
- **Formato das respostas (sucesso):** **divergente** — usam envelope `{message, dados}`/`{mensagem, dados}` (POST/PUT/DELETE) e, na listagem, ora array direto ora `{produtos:…}`. O plano exige **JSON direto**, sem envelopes. ✘
- **Formato dos erros:** **divergente** — usam `{"erro":…}`/`{"error":…}` em vez de `{"detail": …}`. ✘
- **Códigos HTTP:** **divergentes** — POST retorna 200 (deveria 201); DELETE retorna 200 com corpo (deveria 204); GET por ID inexistente: Express 404 / FastAPI 200 (no `aula3a`). ✘
- **Validações:** **insuficientes** — não atendem a regra completa (nome 2–100 + trim; preço com até 2 casas decimais). ✘

---

## 8. As cinco decisões de contrato

### 1. Formato exato das respostas da API (JSON direto, sem envelopes)
- **Compatível:** modelagem `Produto(id, nome, preco)`; os GET de listagem/detalhe em `aula2b`, `aula4`, `aula5` já retornam array/objeto direto.
- **Diverge:** POST/PUT retornam `{message, dados}`; DELETE retorna `{message}`; `aula3b` retorna `{produtos:…}`. Precisará alterar a resposta de **POST, PUT e DELETE** para JSON direto (POST→201 com objeto; DELETE→204 vazio).

### 2. Formato dos erros (`detail`)
- **Compatível:** nada (nenhum arquivo usa `detail`).
- **Diverge:** Express usa `{error, …}`; FastAPI usa `{"erro",…}` no `aula3a` e erros Pydantic padrão (estrutura própria) no `aula5`/`aula6`. Precisará adotar `{"detail": …}` e `{"detail": {campo: msg}}`.

### 3. Validação de `nome` e `preco`
- **Compatível:** validação básica já existe (nome não vazio; preço > 0) em `aula5` Express e Pydantic `Field` no FastAPI.
- **Diverge:** faltam `trim`, mínimo 2 e máximo 100 caracteres no nome; no preço, falta o teto de **2 casas decimais** (`max_digits`/formato numérico). Afeta `aula5`/`aula6` de ambas.
- **Express (afetado):** `aula5_crud_completo_…js`. **FastAPI (afetado):** `aula5` e `aula6` (via Pydantic `Field`).

### 4. Formato do `PUT` (atualização completa)
- **Compatível em conceito:** PUT já trata o recurso por inteiro (`nome`+`preco`) no `aula4`/`aula5` de ambas.
- **Diverge:** formato de resposta (envelope). Após padronização, PUT deve retornar 200 + objeto direto.

### 5. Convenção `NN-api-completa`
- **Compatível:** nada. Nenhum arquivo usa `NN-…` nem `-api-completa` (usam `aulaN_…`). **Diverge 100%.**
- Precisarão ser **renomeados/reorganizados** para `NN-descricao` e o final para `NN-api-completa`. (Mais detalhe na seção 13.)

### 6. Paginação (contrato comum fechado)

**Decisão fechada** no plan (além das cinco anteriores). As três APIs devem expor o mesmo contrato:

```text
GET /api/produtos/?page=1
GET /api/produtos/?page=2
GET /api/produtos/?page=2&page_size=20
```

- `page`: página solicitada (base 1);
- `page_size`: opcional, itens por página (**padrão 10**, **máximo 100**);
- resposta paginada com estrutura **exata**:

```json
{
    "page": 1,
    "page_size": 10,
    "total_pages": 5,
    "results": [
        {
            "id": 1,
            "nome": "Notebook",
            "preco": 3500.00
        }
    ]
}
```

- **Não** serão usados `count`, `next`, `previous`, `total` etc. (nada além de `page`, `page_size`, `total_pages`, `results`).

**Estado atual vs. decisão:**
- **Divergente em ambas:** Express e FastAPI usam `pagina`/`por_pagina`, retornam **array direto** (sem `page`/`page_size`/`total_pages`/`results`) e com **semântica de página divergente entre si** (Express trata `pagina` como offset; FastAPI trata como página 0-based).
- **Django:** paginação **ainda não configurada** no código (nem `PAGE_SIZE`, nem `DEFAULT_PAGINATION_CLASS`); a Aula 13 do README descreve justamente o formato `page/page_size/total_pages/results`, alinhado ao contrato — falta apenas implementar.
- **Alteração futura:** Express e FastAPI deverão adotar `page`/`page_size` e retornar o envelope `{page, page_size, total_pages, results}` na listagem; Django deverá ser configurado para o mesmo formato.

---

## 9. Análise da documentação

- **README "principal"** é o `desweb2/README.md` (Aulas 1–14). Está organizado por **Nº de aula** ("Aula 1", "Aula 2", …), com seções internas — próximo do que o plano pede (capítulos numerados + seções), mas nomeado por "aulas" e não por "capítulos conceituais", e **não possui um índice/estrutura conceitual explícita** (Fundamentos, API-base, DRF, Vue…).
- **READMEs por subprojeto** (`express-bsi4`, `fastapi-bsi4`, `django-bsi4`): **diferentes do plano** ("Não haverá um README independente para cada aula"). Precisam ser incorporados/reorganizados num README único, ou convertidos em "código de apoio" sem funções de README.
- **Documentação fragmentada:** o conteúdo conceitual está espalhado por esses READMEs e `docs/plan.md` (plano separado). O plano quer **um único README** com os capítulos (o `plan.md` pode permanecer como documento de decisão, mas a documentação didática deve ser unificada).
- **Links:** `desweb2/README.md` referencia repositórios externos (`django-drf-tutorial`, `song-vue`, etc.) e guias — ok, mas alguns links apontam para repositórios de aula ("github.com/marrcandre/…") que ainda não existem como conteúdo aqui.
- **Coerência com os arquivos de código:** **parcial.** As **Aulas 13 (paginação) e 14 (validação)** descrevem funcionalidades **que não estão implementadas** no código atual do Django (`settings.py` não configura paginação; `serializers.py` não tem `validate_*`). A **Aula 5/6 do README sobre FastAPI** descreve paginação por `pagina/por_pagina` que difere do contrato. O README está **à frente do código** em alguns pontos e **behind** em outros (ex.: rota `/api/produtos/` descrita no plano não está nos exemplos).

---

## 10. Análise do Django

**Estado atual (mais avançado que a API-base):**

- **Modelos:** `Categoria(nome, descricao)` e `Produto(nome, descricao, estoque, preco, criado_em, atualizado_em, categoria FK → Categoria)`.
  - Campos que **extrapolam** o model base (`id/nome/preco`): `descricao`, `estoque`, timestamps e o relacionamento `categoria`. ✅ previsto como "evolução".
- **Relacionamentos:** `ForeignKey(Categoria, on_delete=PROTECT, null/blank, related_name='produtos')`.
- **Serializers:** `ProdutoSerializer(meta fields='__all__')`, `CategoriaSerializer`. Nenhuma validação customizada ainda.
- **ViewSets:** `ProdutoViewSet` e `CategoriaViewSet` (`ModelViewSet`).
- **Routers:** `DefaultRouter` com `categorias` e `produtos` prefixados por `api/` → `/api/produtos/`, `/api/categorias/` (**com barra final e prefixo `/api`** — já segue o padrão-alvo do contrato). ✔
- **Filtros:** `django-filter` com `ProdutoFilter` (`preco_minimo`, `preco_maximo`, `estoque`) e `filterset_fields` para `estoque` (`exact/gte/lte`).
- **Busca:** `search_fields = ['nome', 'categoria__nome']`.
- **Ordenação:** `ordering_fields = ['nome','preco','atualizado_em']`, default `-atualizado_em`.
- **Paginação:** **NÃO configurada** (nem `PAGE_SIZE`, nem `DEFAULT_PAGINATION_CLASS`). A Aula 13 descreve isso, mas o código ainda não implementa.
- **Documentação:** drf-spectacular configurado (`/api/schema`, `/api/docs`, `/api/redoc`).
- **Admin:** registrado e estruturado (`list_display`, `search_fields`, `list_filter`).
- **API resultante:** `/api/produtos/` e `/api/categorias/` com CRUD completo + filtros + busca + ordenação + docs. **Sem paginação e sem validação customizada** (embora descritas no README).

**Onde o Django já está à frente (potencial de "evolução didática" posterior):** campos extras + `Categoria` + relacionamento + ORM/SQLite + admin. Esses recursos **não devem ser removidos nem replicados** no Express/FastAPI (decisão explícita do plano). Servirão para demonstrar modelagem/abstração mais tarde.

**Atenção:** a **validação** e a **paginação** descritas nas Aulas 13–14 **ainda não existem no código** — isso deve ser harmonizado quando o Django for trabalhado (não agora, nesta fase de análise).

---

## 11. Análise do Vue

- **Não existe** — o diretório `vuejs-bsi4/` está **vazio** (nenhum arquivo).
- Nenhuma tela, componente ou chamada HTTP presente.
- **Impacto futuro:** o Vue consumirá `/api/produtos/` (respostas em **JSON direto**, sem envelopes) e, idealmente, os padrões de erro `detail`, filtros `preco_minimo/max_preco`, busca `search`, ordenação `ordering` e paginação que forem consolidados. Como hoje o contrato Express/FastAPI ainda usa envelopes e rotas sem `/api/`, **a consolidação do backend deve preceder o Vue** (exatamente como o plano orienta). Nenhum trabalho de Vue será proposto nesta fase.

---

## 12. Proposta de sequência didática (Express + FastAPI por conceito)

Baseada nos arquivos reais e no plano, dividindo a atual `aula5` e criando lacunas:

```
01 — API básica (GET lista + rotas)      → Express aula2a/aula2b  → FastAPI aula2a/aula2b
02 — GET por ID                          → Express aula3a          → FastAPI aula3a
03 — POST                                → Express aula2b/aula4    → FastAPI aula2b/aula4
04 — PUT                                 → Express aula4           → FastAPI aula4
05 — DELETE                              → Express aula4           → FastAPI aula4
06 — Validação (nome 2–100+trim; preço>0 e ≤2 casas) → Express aula5 → FastAPI aula5/aula6
07 — Filtros (preco_minimo/preco_maximo) → Express aula3b/aula5    → FastAPI aula3b/aula5
08 — Busca (search)                      → (criar)                 → (criar)
09 — Ordenação (ordering, -preco)        → Express aula5           → FastAPI aula5/aula6
10 — Paginação (page/page_size/total_pages/results) → Express aula5 → FastAPI aula5/aula6
11 — Persistência JSON                   → (criar Express)         → FastAPI aula6
12 — API completa (NN-api-completa)      → (criar)                 → (criar)
```

Observações:
- A ordem conceitual segue o plano (§8/§25). A **busca** (`search`) entra entre filtros e ordenação, conforme o plano.
- **Persistência JSON no Express** deve espelhar o conteúdo didático de `aula6` do FastAPI.
- O arquivo final `NN-api-completa` de cada tecnologia deve consolidar todos os conceitos já padronizados ao contrato.

---

## 13. Proposta de convenção de arquivos

Seguindo o plano (`NN-descricao` para progressivos, `NN-api-completa` para o final):

```
Express (.js)                            FastAPI (.py)
01-api-basica.js                         01-api-basica.py
02-get-id.js                             02-get-id.py
03-post.js                               03-post.py
04-put.js                                04-put.py
05-delete.js                             05-delete.py
06-validacao.js                          06-validacao.py
07-filtros.js                            07-filtros.py
08-busca.js                              08-busca.py
09-ordenacao.js                          09-ordenacao.py
10-paginacao.js                          10-paginacao.py
11-persistencia-json.js                  11-persistencia-json.py
12-api-completa.js                       12-api-completa.py
```

Princípios da convenção:
- **Intermediários:** `NN-descricao` (número de 2 algarismos + hífen + descrição minúscula, sem espaço).
- **Express/FastAPI com numeração idêntica** quando representam o mesmo conceito (paralelismo didático).
- **Finais:** `NN-api-completa` (mesmo `NN` nas duas tecnologias), **substituindo** os sufixos atuais `_crud_completo`, `-completo`, `-final`.
- **Compartilhados:** dados de apoio fora da numeração (`produtos.json`), permanecendo em cada subpasta; nada de arquivo compartilhado entre tecnologias (mantém independência).
- Nenhuma renomeação será feita agora — apenas proposta.

**Mapeamento da renomeação (se/além do plano):** as atuais `aulaX*` seriam renomeadas. Ex. `aula2a.js`→`01-api-basica.js`; `aula4_crud_completo.js` e `aula5_…` devem ser **divididas/reorganizadas** em etapas por conceito (03-post, 04-put, 05-delete, 06-validacao, …) e não copiadas um-para-um.

---

## 14. Lacunas identificadas

### Crítica
1. **Rota fora do contrato:** nenhum exemplo usa `/api/produtos/` (falta prefixo `/api` e barra final). Bloqueia a padronização do contrato e o consumo futuro pelo Vue.
2. **Busca (`search`) inexistente** em Express e FastAPI — conceito previsto no plano, sem nenhum arquivo.
3. **Persistência JSON inexistente no Express** — correspondência de `aula6` do FastAPI faltando.
4. **Formato de resposta usando envelopes** (`{message, dados}`) — viola a decisão central de "JSON direto".

### Importante
5. **Formato de erro** divergente (`{erro}/{error}` em vez de `{detail}`).
6. **Códigos HTTP incorretos** (POST deve ser 201; DELETE deve ser 204; GET-id inexistente deve ser 404 uniformemente).
7. **Validação incompleta** (nome sem 2–100/trim; preço sem teto de 2 casas decimais).
8. **Ordenação divergente** do contrato (`ordenar_por/ordem` em vez de `ordering` com `-`).
9. **Paginação divergente do contrato fechado:** o padrão comum do plan (`page`/`page_size`/`total_pages`/`results`, padrão 10, máx. 100) **não está implementado**. Express e FastAPI usam `pagina`/`por_pagina` com semântica diferente entre si.
10. **Nomes de parâmetros inconsistentes** (`id`, `item_id`, `id_produto`).

### Pode aguardar
11. Convenção `NN-…`/`NN-api-completa` (renomeação — só após o mapa estar estável).
12. Unificação em README único (reorganização editorial, pode vir depois do código).
13. **Paginação no Django** (contrato já está definido no plan e descrito na Aula 13; implementar quando o Django for trabalhado na evolução).
14. Validação customizada no Django (idem).
15. Vue (depende da consolidação do backend).

---

## 15. Plano de trabalho proposto

**FASE 1 — Alinhamento Express/FastAPI por conceito**
- Objetivo: transformar a granularidade por "aula" em etapas por conceito (`01…11`), espelhadas nas duas tecnologias.
- Arquivos: todos os `.js` do Express e `.py` do FastAPI.
- Alterações: divisão da `aula5`; criação da parte de **busca** e de **persistência** no Express.
- Dependências: leitura do mapa (§4).
- Riscos: quebrar a correspondência de numeração; volume de edições.

**FASE 2 — Padronização do contrato**
- Objetivo: aplicar rotas `/api/produtos/`, JSON direto, `detail`, códigos 201/204, validação completa, `ordering`, e **aplicar o padrão comum de paginação já fechado** (`page`/`page_size`/`total_pages`/`results`, padrão 10, máx. 100).
- Arquivos: etapas avançadas (validacao, filtros, busca, ordenacao, paginacao) de ambas.
- Alterações: rotas, respostas de POST/PUT/DELETE, formato de erro, validações.
- Dependências: Fase 1 (arquivos reorganizados).
- Riscos: mudança é transversal; exigir testes em cada etapa.

**FASE 3 — Reorganização/renomeação dos arquivos progressivos**
- Objetivo: aplicar `NN-descricao` e criar `NN-api-completa` (12) nas duas tecnologias.
- Arquivos: renomeações em `express-bsi4` e `fastapi-bsi4`.
- Alterações: apenas nomes/estrutura + atualização dos READMEs de execução.
- Dependências: Fases 1 e 2 (conteúdo estável).
- Riscos: referências em versões antigas dos READMEs.

**FASE 4 — Atualização do README**
- Objetivo: um único README com capítulos numerados e seções, incorporando os READMEs por subprojeto.
- Arquivos: `desweb2/README.md` e READMEs de `express/fastapi/django`.
- Alterações: reorganização editorial, referências aos novos arquivos `NN-…`.
- Dependências: Fase 3 (nomes dos arquivos).
- Riscos: conteúdo "à frente" das aulas de Django (paginação/validação) precisa ser marcado como futuro.

**FASE 5 — Implementação das lacunas**
- Objetivo: fechar busca, persistência no Express e padrão de paginação.
- Arquivos: novas etapas 08 (busca) e 11 (persistência Express).
- Dependências: Fases 1–2.
- Riscos: compatibilidade do formato de retorno.

**FASE 6 — Revisão do Django**
- Objetivo: harmonizar paginação e validação (já documentadas) sem reduzir o modelo; aproveitar a evolução (Categoria etc.).
- Arquivos: `settings.py` (paginação), `serializers.py` (validação).
- Dependências: contrato do Fase 2 (para coerência do retorno).
- Riscos: não artificialmente reduzir para igualar ao Express/FastAPI.

**FASE 7 — Preparação do consumo da API**
- Objetivo: validar o contrato consolidado com Postman/curl/Swagger e definir o padrão consumido pelo Vue.
- Arquivos: docs + exercícios.
- Dependências: Fases 2 e 6.
- Riscos: divergências residuais de resposta.

**FASE 8 — Vue**
- Objetivo: construir o cliente GET→CRUD→filtros/busca.
- Arquivos: projeto novo em `vuejs-bsi4/`.
- Dependências: backend consolidado (Fases 2, 6, 7).
- Riscos: mudanças tardias no contrato afetam o front.

---

## 16. Conclusão (respostas objetivas)

1. **Em que estado estão Express e FastAPI?** Em estágio de exemplos progressivos por "aula", funcionalmente completos para CRUD+filtro+ordenação+paginação+validação em memória, mas fora do contrato (`/api/produtos/`, JSON direto, `detail`, 201/204) e com rotas/nomes de parâmetros divergentes. FastAPI é um pouco mais adiantado (documentação automática + persistência JSON).

2. **Até onde cada um chegou?** Express: CRUD + validação + filtro + ordenação + paginação em memória (`aula5`), sem busca e sem persistência. FastAPI: tudo isso + **persistência em JSON** (`aula6`), com validação via Pydantic e docs automática; também sem busca.

3. **Mapa de correspondência:** alinhados em API-base/GET-lista, GET-por-ID (com divergência de status), filtros, CRUD, validação, ordenação e paginação (com semântica divergente); **FastAPI adiantado em persistência**; **busca ausente em ambos**.

4. **Principais divergências:** (a) rotas sem `/api/produtos/`; (b) respostas com envelopes vs. JSON direto; (c) formato de erro (`error/erro` vs. `detail`); (d) códigos HTTP (POST 200 vs 201; DELETE 200 vs 204; GET-id 200 no FastAPI `aula3a`); (e) paginação com semântica diferente; (f) ordenação `ordenar_por/ordem` vs. `ordering`; (g) nomes de parâmetros; (h) validação incompleta; (i) persistência/mdocs presentes só no FastAPI.

5. **O que preservar:** os arquivos progressivos de ambas (base didática), o emparelhamento Express/FastAPI, a validação manual vs. Pydantic, o modelo `Produto(id,nome,preco)`, filtros `min_preco/max_preco`, docs automática do FastAPI, todo o material do Django (`Categoria`, filters, viewsets, routers, spectacular) e o `plan.md`.

6. **O que reorganizar:** a granularidade por "aula" → por "conceito"; a mistura de conceitos na `aula5`; os READMEs por subprojeto → README único.

7. **Arquivos que provavelmente precisarão ser criados:** `08-busca.*` (Express e FastAPI), `11-persistencia-json.js` (Express), `12-api-completa.*` (Express e FastAPI) e, futuramente, projeto Vue.

8. **Arquivos a renomear para `NN-api-completa`:** os finais de cada tecnologia (Express e FastAPI) → ex. `12-api-completa.js/.py`; os intermediários → `NN-descricao` em ambas (ver tabela da seção 13), substituindo a nomenclatura `aulaX…` e `_crud_completo`.

9. **Primeira alteração a fazer:** **FASE 1 + FASE 2**, ou seja, num primeiro momento **padronizar rotas (`/api/produtos/`), formato de resposta (JSON direto em POST/PUT/DELETE), erro `detail`, códigos 201/204 e validação completa**, começando pela revisão/criação da **persistência e busca** e pela **divisão da `aula5` em etapas por conceito**. Em termos de "primeiro passo concreto": **criar a etapa de busca e a persistência no Express para equilibrar as duas tecnologias**, e só depois padronizar o contrato.

10. **Problemas técnicos/pedagógicos a resolver antes de mexer nos códigos:**
    - **Padrão comum de paginação — JÁ FECHADO** no plan (`page`/`page_size`/`total_pages`/`results`, padrão 10, máx. 100). Não é mais uma decisão em aberto; resta **aplicá-lo** em Express, FastAPI e Django.
    - **Alinhar o nome dos parâmetros** de rota (`id`/`item_id`/`id_produto`) e a chave de ordenação (`ordering` com `-`).
    - **Resolver a divergência de status do GET-por-ID inexistente** no FastAPI `aula3a` (hoje retorna 200 com `{erro}`, deve ser 404 `{detail}`).
    - **Decidir como tratar a "evolução" do Django** sem reduzir o modelo, e harmonizar a **paginação/validação que já são documentadas nas Aulas 13–14 mas ainda não implementadas**.
    - **Definir a política do README único** (numeração de capítulos conceituais) e como integrar os READMEs por subprojeto, para que a documentação não fique "à frente" do código.

---

## 17. Comparativo com o plan.md (decisões fechadas) — status atual

Classificação do estado atual de cada item do planejamento, com base no código real e no contrato fechado (incluindo a paginação):

| # | Decisão fechada (plan) | Status | Detalhe |
|---|---|---|---|
| 1 | Modelo `Produto(id, nome, preco)` | **Correto ✅** | Express e FastAPI usam id/nome/preco. Django extrapola (evolução prevista, não reduzir). |
| 2 | URL-base `/api/produtos/` | **Divergente ✘** | Nenhum exemplo usa prefixo `/api` nem barra final (usam `/produtos`). |
| 3 | CRUD completo | **Parcial/Divergente** | CRUD existe, mas com respostas em envelope e códigos HTTP errados (POST 200, DELETE 200). |
| 4 | `PUT` como atualização completa | **Parcial ✅** | Já trata o recurso por inteiro; falta padronizar a resposta (objeto direto, 200). |
| 5 | Ausência de `PATCH` na etapa inicial | **Correto ✅** | Nenhum exemplo implementa `PATCH` (coerente). |
| 6 | Validação `nome` (trim, 2–100) e `preco` (>0, ≤2 casas) | **Parcial ✘** | Validação básica existe, mas faltam `trim`/2–100 no nome e teto de 2 casas no preço. |
| 7 | Filtros (`preco_minimo`/`preco_maximo`) | **Correto ✅** | Nomes coincidem com o plan nas duas tecnologias. |
| 8 | Busca (`search`) | **Faltando ✘** | Inexistente em Express e FastAPI; implementada no Django. |
| 9 | Ordenação (`ordering`, com `-`) | **Divergente ✘** | Usam `ordenar_por`/`ordem` em vez de `ordering`/`-campo`. |
| 10 | Paginação (`page`/`page_size`/`total_pages`/`results`) | **Divergente ✘** | Usam `pagina`/`por_pagina`, array direto e semântica divergente entre si; Django sem paginação no código. |
| 11 | Contrato de respostas (JSON direto, sem envelopes) | **Divergente ✘** | POST/PUT/DELETE usam `{message, dados}`; listagem varia entre array e `{produtos:…}`. |
| 12 | Contrato de erros (`detail`) | **Divergente ✘** | Usam `{erro}`/`{error}`; FastAPI usa estrutura Pydantic própria. |
| 13 | Códigos HTTP (200/201/204/400/404) | **Divergente ✘** | POST não retorna 201; DELETE não retorna 204; GET-id inexistente divergente (200 vs 404). |
| 14 | Persistência (memória → JSON) | **Parcial** | Memória nas duas; JSON só no FastAPI (`aula6`); falta no Express. |

## 18. Alterações de código necessárias posteriormente

### Express (`express-bsi4/`)
- Adotar rotas `/api/produtos/` e `/api/produtos/:id/` (prefixo `/api` + barra final).
- Padronizar o nome do parâmetro de rota para `id`.
- Retornar **JSON direto** em POST (201), PUT (200) e DELETE (204 sem corpo).
- Usar `detail` no erro (404 do GET por ID inexistente e nas validações).
- Completar validação: `trim`, nome 2–100, preço >0 e ≤2 casas decimais.
- Trocar a ordenação para `ordering` (aceitar `-campo`).
- Trocar a paginação para `page`/`page_size` e retornar `{page, page_size, total_pages, results}`.
- Adicionar **busca (`search`)**.
- Adicionar **persistência JSON** (contraparte de `aula6` do FastAPI).
- Dividir `aula5` em etapas por conceito; renomear para `NN-descricao` e criar `NN-api-completa`.
- Remover o envelope `{produtos:…}` da listagem em `aula3b`.

### FastAPI (`fastapi-bsi4/`)
- Adotar rotas `/api/produtos/` e `/api/produtos/{id}/` (prefixo `/api` + barra final).
- Padronizar o nome do parâmetro de rota (`item_id`/`id_produto` → `id`).
- Corrigir o GET por ID inexistente no `aula3a` (hoje retorna 200 com `{"erro":…}` → 404 com `{"detail": "Produto não encontrado."}`).
- Retornar **JSON direto** em POST (201), PUT (200) e DELETE (204 sem corpo).
- Padronizar os erros de validação Pydantic para o formato `{"detail": {campo: msg}}`.
- Completar a validação: `trim` e nome 2–100; preço >0 e ≤2 casas decimais.
- Trocar a ordenação para `ordering` (aceitar `-campo`).
- Trocar a paginação para `page`/`page_size` e retornar `{page, page_size, total_pages, results}`.
- Adicionar **busca (`search`)** (em `aula5`/`aula6`).
- Dividir `aula5`/`aula6` em etapas por conceito; renomear para `NN-descricao` e criar `NN-api-completa`.

### Django (`django-bsi4/`)
- **Não reduzir** o modelo (`Categoria`, campos extras, relacionamento) — é a evolução prevista.
- Implementar **paginação** (`DEFAULT_PAGINATION_CLASS` + `PAGE_SIZE`) retornando `{page, page_size, total_pages, results}`, alinhada ao contrato fechado (a Aula 13 já descreve o formato).
- Implementar **validação customizada** nos serializers (`validate_<campo>`, conforme Aula 14), mantendo regras equivalentes (nome 2–100, preço >0 e ≤2 casas).
- Quando a evolução for trabalhada, alinhar filtros/ordenação já existentes ao contrato comum.

## 19. Etapas que podem ser preservadas sem alteração

Classificação (aplicável ao código e à documentação, sem modificá-los agora):

| Etapa | Tecnologia | Por que preservar |
|---|---|---|
| `01` API-base (GET lista) | Express `aula2a/2b` · FastAPI `aula2a/2b` | Cumpre o 1.º conceito; só exige ajuste de rota/resposta, não reescrita |
| GET por ID | Express `aula3a` · FastAPI `aula3a` | Lógica ok; Exige apenas corrigir status/erro do FastAPI e formato de rota |
| Filtros (`min_preco/max_preco`) | Express `aula3b/5` · FastAPI `aula3b/5` | Parâmetros já coincidem com o plan |
| Validação manual vs. Pydantic | Express `aula5` · FastAPI `aula5/6` | É exatamente a comparação pedagógica desejada |
| Ordenação/paginação (conceito) | Express `aula5` · FastAPI `aula5/6` | Conceito presente; apenas o contrato/parâmetros mudam |
| Persistência JSON | FastAPI `aula6` | Modelo a ser replicado no Express |
| Contrato/evolução do Django | Django (models, filters, viewsets, routers, spectacular) | Não deve ser reduzido; é a evolução didática |
| Docs automática (`/docs`, `/redoc`) | FastAPI | Preservar |
| `Produto(id, nome, preco)` como base | Todas | Campo do contrato |

---

# Próxima etapa de implementação

Esta seção **não implementa nada** — apenas indica a **ordem recomendada das próximas alterações de código**, respeitando a metodologia:

```text
Conceito
   ↓
Express
   ↓
FastAPI
   ↓
Comparação
   ↓
Próximo conceito
```

Cada etapa conceitual é feita primeiro no Express e imediatamente depois no FastAPI, comparando-se os resultados antes de avançar. A ordem abaixo aplica, em cada etapa, também os ajustes de contrato já fechados (rotas `/api/produtos/`, JSON direto, `detail`, códigos 201/204/404, validação completa, `ordering`, paginação `page`/`page_size`/`total_pages`/`results`).

1. **Rota e resposta da API básica** — adequar `/api/produtos/`, JSON direto e códigos HTTP. → Express (`aula2a/2b`) · FastAPI (`aula2a/2b`). *(incorpora contrato de rota/resposta/status)*
2. **GET por ID** — uniformizar 404 com `detail`, padronizar parâmetro de rota e barra final. → `Express aula3a` · `FastAPI aula3a`.
3. **POST** — retorno 201 em JSON direto. → Express · FastAPI.
4. **PUT** — atualização completa, resposta 200 em JSON direto. → Express · FastAPI.
5. **DELETE** — retorno 204 sem corpo. → Express · FastAPI.
6. **Validação** — `trim`, nome 2–100; preço >0 e ≤2 casas; erro `detail` por campo. → Express (manual) · FastAPI (Pydantic).
7. **Filtros** — `preco_minimo`/`preco_maximo`(já ok, apenas rotas/retorno). → Express · FastAPI.
8. **Busca (`search`)** — criar em ambas. → Express · FastAPI.
9. **Ordenação** — adotar `ordering` com `-campo`. → Express · FastAPI.
10. **Paginação** — adotar `page`/`page_size`/`total_pages`/`results` (padrão 10, máx. 100). → Express · FastAPI.
11. **Persistência JSON** — criar no Express (contraparte de `aula6`). → Express · FastAPI (já pronto).
12. **API completa** — criar `NN-api-completa` em cada tecnologia e refatorar a numeração para `NN-descricao`.
13. **Renomeação dos arquivos progressivos** — aplicar `NN-descricao` nas duas (sem alterar conteúdo).
14. **Django** — implementar paginação e validação customizada conforme contrato; usar a evolução (Categoria etc.) como etapa didática posterior.
15. **README único** — reorganização editorial em capítulos numerados (após a numeração dos arquivos estar estável).
16. **(Futuro) Consumo da API + Vue.js** — somente após a consolidação do backend.

> Nota: itens 14–16 vêm depois porque dependem do backend consolidado; seguem o plano (README/DRF/Vue fora do escopo progressivo Express↔FastAPI desta ordem).

---

## Resumo das principais divergências (atualizado)

- **Rotas**: nenhuma usa `/api/produtos/` (sem `/api`, sem barra final). Express `/produtos`, FastAPI `/produtos`.
- **Respostas**: POST/PUT/DELETE com envelope `{message, dados}`; listagem ora array direto ora `{produtos:…}` — contrato pede JSON direto.
- **Erros**: `{erro}`/`{error}` (Express/FastAPI) em vez de `{"detail": …}`; FastAPI `aula3a` retorna 200 com `{erro}` em vez de 404.
- **Códigos HTTP**: POST não retorna 201; DELETE não retorna 204.
- **Validação**: incompleta (falta trim/nome 2–100 e teto de 2 casas no preço).
- **Ordenação**: `ordenar_por`/`ordem` em vez de `ordering`/`-campo`.
- **Paginação**: `pagina`/`por_pagina` divergentes entre si; contrato fechado é `page`/`page_size`/`total_pages`/`results` (não implementado).
- **Busca**: ausente em Express e FastAPI (presente no Django).
- **Persistência JSON**: só no FastAPI (`aula6`); faltando no Express.
- **Arquivos**: nomenclatura `aulaN_…` não segue `NN-descricao`; não existe `NN-api-completa`.
- **Django**: paginação e validação ainda não implementadas (descritas na doc); modelo já está além da API-base (Categoria etc.), o que é evolução prevista.
- **README**: por subprojeto e por "aulas"; plan pede README único.

### Confirmação

**Nenhum código foi alterado.** A única modificação realizada nesta tarefa foi a atualização do documento `desweb2/docs/analise-estado-atual.md`. Nenhum arquivo `.js`, `.py`, `.json`, README, `plan.md` ou configuração foi modificado.
