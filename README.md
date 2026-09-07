# Desenvolvimento Web II — APIs com Express, FastAPI e Django

**1. Objetivo do tutorial**

Este material ensina a **construir uma API REST**, do zero até uma versão completa e evolutiva, utilizando **três implementações da mesma API**:

* **Express** (Node.js / JavaScript);
* **FastAPI** (Python);
* **Django + Django REST Framework (DRF)** (Python).

O objetivo **não** é apenas dominar múltiplos frameworks, e sim compreender o que é uma **API HTTP/REST** e perceber que frameworks com diferentes níveis de abstração são **formas distintas de implementar o mesmo contrato**.

---

**2. Ideia central: Express × FastAPI × Django**

> Express, FastAPI e Django REST Framework implementam a mesma API. A forma de programar e o nível de abstração mudam, mas o contrato HTTP permanece.

Ao longo do curso construímos a mesma API de produtos:
- No **Express**, construímos os manipuladores e validações manualmente sobre Node.js;
- No **FastAPI**, aproveitamos tipagem moderna com Pydantic e documentação OpenAPI nativa;
- No **Django + DRF**, utilizamos o poder de um framework completo com ORM, Migrations, Django Admin, Serializers e ViewSets declarativos.

O que o cliente (navegador, Bruno, aplicativo) envia e recebe é **praticamente idêntico** nas três tecnologias; o que muda é **como** cada ecossistema organiza essa solução.

---

**3. Como usar este tutorial**

O tutorial está estruturado em três grandes momentos:
1. **Aulas 1 a 13:** Construção progressiva e comparada da API base no **Express** e no **FastAPI** (CRUD, validação manual, filtros, busca, ordenação, persistência e paginação).
2. **Aulas 14 a 16 (Parte 6):** Evolução da entidade `Produto` adicionando novos campos (`marca`, `estoque`, `descricao`) e vivenciando o aumento de complexidade manual.
3. **Parte 7 (Aulas 17 a 26):** Construção, a partir do zero, de um único projeto no **Django + DRF**, explorando a sequência:
   ```text
   uv → Django → Model → SQLite + Migrations → Django Admin → DRF → ModelSerializer → ModelViewSet → Router → Swagger/OpenAPI → Validações → Filtros/Busca → Evolução do Produto
   ```

O tutorial **explica os conceitos e a arquitetura**. Nas Partes 1–6, consulte e execute os exemplos nos repositórios correspondentes; na Parte 7, construa o projeto Django progressivamente conforme as instruções.

---

**4. Pré-requisitos**

Você precisa ter instalado:

- **Node.js** (versão 18 ou superior) e **npm** — para o Express;
- **Python** (versão 3.14 ou superior) — para o FastAPI e Django;
- **uv** (gerenciador moderno de pacotes e ambientes Python) — para o Django + DRF;
- **pip** — instalador padrão do Python;
- **Bruno** (aplicação de desktop) — cliente HTTP para testar a API.

---

**5. Onde está o código (repos externos)**

> **Este repositório (`desweb2`) contém o material didático unificado.** O código das Partes 1–6 está organizado nos repositórios de Express e FastAPI. Na Parte 7, o aluno cria o projeto `django-bsi4` conforme as instruções deste README:
>
> - **Express (Node.js / JavaScript):** https://github.com/marrcandre/express-bsi4
> - **FastAPI (Python):** https://github.com/marrcandre/fastapi-bsi4

| Repositório   | Tecnologia          | Arquivos / Estrutura      | Porta | Documentação / Coleção |
| ------------- | ------------------- | ------------------------- | ----- | ---------------------- |
| `express-bsi4` | Node.js + Express   | `aula2_*.js` … `aula13_*.js` | 3000  | Coleção Bruno `http/express/` |
| `fastapi-bsi4` | Python + FastAPI    | `aula2_*.py` … `aula13_*.py` | 8000  | Swagger `/docs` e Bruno `http/fastapi/` |
| `django-bsi4`  | Python + Django DRF | Projeto criado progressivamente na Parte 7 | 8000  | Swagger `/api/docs/` e Django Admin `/admin/` |

---

**6. Bruno — cliente HTTP do curso**

**Por que usamos o Bruno**

O **Bruno** é o cliente HTTP oficial da disciplina. Ele permite **guardar as requisições junto com o código** e **versioná-las no repositório**. Assim, cada aula tem uma coleção de requisições que exercita exatamente o que foi construído.

**Executar o servidor ≠ testar a API.** O servidor (a aula que você inicia com `node`, `uvicorn` ou `manage.py runserver`) **processa** as requisições e devolve respostas. O **Bruno** ou a interface **Swagger** enviam essas requisições para você observar o que a API responde.

> **Nota:** As coleções oficiais do Bruno acompanham os repositórios `express-bsi4` e `fastapi-bsi4`. Para o `django-bsi4` (Parte 7), os testes e interações são realizados via Swagger UI (`/api/docs/`) e interface navegável do DRF, ficando a coleção Bruno registrada como evolução futura.

---

**7. Visão geral das partes e aulas**

| Aula | Tema / Conceito      | Parte | Tecnologias |
| ---- | -------------------- | ----- | ----------- |
| 01   | Fundamentos de APIs  | Parte 1 — Fundamentos e primeiros endpoints | Teoria / Bruno |
| 02   | GET de coleção       | Parte 1 | Express × FastAPI |
| 03   | GET por ID           | Parte 1 | Express × FastAPI |
| 04   | POST                 | Parte 2 — CRUD | Express × FastAPI |
| 05   | PUT                  | Parte 2 | Express × FastAPI |
| 06   | DELETE               | Parte 2 | Express × FastAPI |
| 07   | Validação            | Parte 3 — Refinando a API | Express × FastAPI |
| 08   | Filtros              | Parte 3 | Express × FastAPI |
| 09   | Busca                | Parte 3 | Express × FastAPI |
| 10   | Ordenação            | Parte 3 | Express × FastAPI |
| 11   | Persistência em JSON | Parte 4 — Persistência e paginação | Express × FastAPI |
| 12   | Paginação            | Parte 4 | Express × FastAPI |
| 13   | API completa         | Parte 5 — Consolidação | Express × FastAPI |
| 14   | Adicionando marca    | Parte 6 — Complexidade progressiva | Express × FastAPI |
| 15   | Adicionando estoque  | Parte 6 — Complexidade progressiva | Express × FastAPI |
| 16   | Adicionando descrição| Parte 6 — Complexidade progressiva | Express × FastAPI |
| 17   | Conhecendo o Django e criando o projeto | Parte 7 — Django | Django + DRF |
| 18   | Model e banco de dados | Parte 7 | Django + DRF |
| 19   | Django Admin | Parte 7 | Django + DRF |
| 20   | Primeiro endpoint com Django REST Framework | Parte 7 | Django + DRF |
| 21   | OpenAPI e Swagger | Parte 7 | Django + DRF |
| 22   | Validações | Parte 7 | Django + DRF |
| 23   | Filtros | Parte 7 | Django + DRF |
| 24   | Ordenação e busca textual | Parte 7 | Django + DRF |
| 25   | Paginação | Parte 7 | Django + DRF |
| 26   | Exercício: Evoluindo o Produto | Parte 7 | Django + DRF |

---

# 🧭 Parte1 — Fundamentos e primeiros endpoints

---

## 📘 Aula 01 — Fundamentos de APIs

**1. O que é uma API**

**API** (*Application Programming Interface*) é um conjunto de regras que definem **como um sistema
pode conversar com outro**. Uma **API web** é acessível pela internet, normalmente usando o protocolo
**HTTP**.

Quando você abre um aplicativo de clima, ele consulta uma API de um serviço meteorológico. Quando um
site calcula o frete, ele consulta a API de uma transportadora. Em Desenvolvimento Web II, nosso
objetivo é **criar** esse tipo de serviço — uma API que devolve dados de produtos.

**2. Cliente, servidor, requisição e resposta**

- **Cliente** — quem faz a requisição (navegador, aplicativo, ou o próprio Bruno).
- **Servidor** — quem recebe, processa e devolve a resposta (no nosso caso, uma API).
- **Requisição (request)** — o que o cliente envia: método, URL e, às vezes, um corpo.
- **Resposta (response)** — o que o servidor devolve: um status e, em geral, um corpo JSON.

```text
Cliente
   │
   │ HTTP Request (método + URL + corpo)
   ▼
Servidor / API
   │
   │ HTTP Response (status + corpo)
   ▼
Cliente
```

Esse ciclo "pedido → resposta" é a base de tudo que faremos no curso.

**3. HTTP**

O **HTTP** é o protocolo usado para essa comunicação. Uma requisição HTTP é composta, basicamente,
por:

- **método** — a intenção da operação;
- **URL** — para onde a requisição vai;
- **parâmetros** — dados extras na URL (ex.: `/api/produtos/1`, `?search=mouse`);
- **headers** — informações de controle (breve agora; usaremos pouco no curso);
- **corpo (body)** — os dados enviados (em POST/PUT).

O servidor responde com um **status code** (código do resultado) e, em geral, um corpo. Os principais
deste curso:

| Código | Significado                  | Quando aparece |
| ------ | ---------------------------- | -------------- |
| `200`  | Sucesso (leitura/atualização)| Aulas 2–5      |
| `201`  | Recurso criado                | Aula 4         |
| `204`  | Sucesso sem corpo (exclusão) | Aula 6         |
| `400`  | Requisição inválida           | Aula 7         |
| `404`  | Recurso não encontrado        | Aula 3         |
| `500`  | Erro interno do servidor      | —              |

Os métodos principais do curso são:

- **GET** — ler dados (Aulas 2 e 3);
- **POST** — criar (Aula 4);
- **PUT** — atualizar por completo (Aula 5);
- **DELETE** — excluir (Aula 6).

> **PATCH** representa atualização **parcial** e faz parte do REST. Nesta sequência usamos o **PUT**
> para atualização completa; o PATCH fica para depois.

**4. JSON**

**JSON** é o formato de dados mais usado em APIs web. Permite representar objetos e listas:

```json
{ "id": 1, "nome": "Notebook", "preco": 3500 }
```

```json
[ { "id": 1, "nome": "Notebook", "preco": 3500 },
  { "id": 2, "nome": "Mouse", "preco": 80 } ]
```

No curso, **JSON é o formato de dados das nossas APIs**: a API recebe e devolve JSON.

**5. REST — ideia principal**

**REST** é um conjunto de boas práticas para organizar APIs web. As principais ideias:

- os dados são organizados em **recursos**;
- cada recurso é identificado por uma **URL** (ex.: `/api/produtos/1`);
- as **operações** sobre o recurso são feitas com **métodos HTTP** (GET, POST, PUT, DELETE);
- o recurso é **representado** em um formato (em geral, **JSON**).

**6. Framework ≠ protocolo**

- **HTTP** é o **protocolo**: define como cliente e servidor conversam (métodos, status, URLs).
- **REST** é uma **forma de organizar** APIs sobre HTTP.
- **Express** é um **framework/ecossistema** para Node.js (JavaScript).
- **FastAPI** é um **framework** para Python.

Os dois frameworks podem implementar o **mesmo contrato HTTP** — mesmo endpoint, mesmos métodos,
mesmas respostas:

```text
                CONTRATO HTTP
       /api/produtos/ + métodos + respostas
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
          Express               FastAPI
          Node.js                Python
             │                     │
             └──────────┬──────────┘
                        ▼
                  mesmo cliente
```

> **O framework muda a forma de implementar. O contrato HTTP permanece.**

**7. Conhecendo os repositórios**

O **código-fonte** das aulas fica nos repositórios de cada tecnologia (o `desweb2` guarda o material
didático). Os links oficiais:

- **Express:** https://github.com/marrcandre/express-bsi4
- **FastAPI:** https://github.com/marrcandre/fastapi-bsi4

Cada um tem seu README com instruções de como clonar, instalar e executar. No `express-bsi4/` estão `aula2_*.js` a `aula13_*.js` (porta 3000); no `fastapi-bsi4/`, `aula2_*.py` a `aula13_*.py` (porta 8000). Cada
arquivo é um servidor completo e progressivo.

**8. Preparando o Bruno**

Instale e abra o **Bruno** (veja a [seção 6](#6-bruno--cliente-http-do-curso) com as instruções por sistema). No Bruno, uma **coleção**
é um conjunto de **requisições** organizadas. As requisições podem usar um **ambiente** com a
**URL base** (como `{{baseUrl}}`), e cada uma define **método**, **URL**, **parâmetros**, **corpo** e
mostra **status** e **resposta**.

**9. Primeira prática no Bruno**

Vamos consultar duas **APIs públicas** e observar o formato JSON:

**ViaCEP**

```
GET https://viacep.com.br/ws/89201000/json/
```

**Cotação do dólar**

```
GET https://economia.awesomeapi.com.br/json/last/USD-BRL
```

No Bruno, crie uma requisição `GET` para cada URL acima, execute e observe:

- o **método** usado;
- a **URL** (para onde foi enviado);
- o **status** retornado;
- o **corpo** (o que o servidor respondeu);
- o **JSON** e os **campos** retornados.

**10. O que vem nas próximas aulas**

O curso está organizado em **cinco partes**:

| Parte | Tema                                  | Aulas |
| ----- | ------------------------------------- | ----- |
| Parte 1 | Fundamentos e primeiros endpoints   | 1–3   |
| Parte 2 | CRUD                                 | 4–6   |
| Parte 3 | Refinando a API (validação, filtros, busca, ordenação) | 7–10 |
| Parte 4 | Persistência e paginação             | 11–12 |
| Parte 5 | Consolidação                         | 13    |

Começamos devolvendo uma **lista fixa em memória** e terminamos com uma **API completa com CRUD, validação,
filtros, busca, ordenação, persistência em um arquivo JSON e paginação**. Nas Aulas 2–13, usaremos as **coleções oficiais** do Bruno que já acompanham os repositórios.

---

## 📘 Aula 02 — GET de coleção

**O que vamos aprender**

Como fazer a API responder a uma requisição **`GET`** com uma **lista (coleção)** de produtos. É o
primeiro endpoint da nossa API.

**Antes de programar**

Precisamos escutar o `GET` em um URL e devolver um **JSON com um array de produtos**. As Aulas 2–10
usam **5 produtos em memória** definidos no próprio código — sem banco, sem arquivo, sem
`produtos.json`.

**Express**

- **Arquivo:** `express-bsi4/aula2_api_basica_get_colecao.js`

```js
app.get('/api/produtos/', (req, res) => {
  res.json([ ... ]);   // array de 5 produtos
});
```

`app.get('/rota', manipulador)` registra o que fazer em uma requisição `GET`. O manipulador recebe
`req` (requisição) e `res` (resposta); `res.json(...)` devolve o corpo em JSON.

**Executar:**
```bash
node aula2_api_basica_get_colecao.js
```
Servidor na porta `3000`. Acesse `http://localhost:3000/api/produtos/`.

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula2_api_basica_get_colecao.py`

```python
@app.get("/api/produtos/")
def listar_produtos():
    return produtos   # lista de 5 produtos
```

O decorador `@app.get(...)` registra a rota. A função **retorna** o valor (lista de dicts), e o
FastAPI converge para JSON automaticamente.

**Executar:**
```bash
uvicorn aula2_api_basica_get_colecao:app --reload
```
Servidor na porta `8000`. Documentação automática em [http://localhost:8000/docs](http://localhost:8000/docs).

**Express × FastAPI**

| Conceito            | Express                | FastAPI                 |
| ------------------- | ---------------------- | ----------------------- |
| Definir a rota GET  | `app.get('/api/produtos/')` | `@app.get('/api/produtos/')` |
| Responder JSON      | `res.json(produtos)`   | `return produtos`        |
| Objeto da requisição| `req`, `res` (explícitos) | parâmetros da função (declarativos) |
| Documentação        | não automática          | `/docs` automática       |

**O que pertence ao HTTP:** a URL `/api/produtos/`, o método `GET`, o status `200` e o corpo JSON.

**O que pertence ao framework:** a sintaxe de registrar rota e enviar resposta.

**Por que diferente?** O Express é mais explícito (você manipula `req`/`res`). O FastAPI é mais
declarativo: você chama a função e devolve um valor.

**Contrato HTTP**

| Método | URL                  | Status | Resposta                |
| ------ | -------------------- | ------ | ----------------------- |
| GET    | `/api/produtos/`      | 200    | array JSON de 5 produtos |

**Pratique no Bruno**

> **Agora é sua vez.** Abra a coleção `http/express/` (e depois `http/fastapi/`), selecione o
> ambiente `Local`, inicie a Aula 2 e execute a requisição **Aula 02 → 01 - listar todos**. Observe
> método, URL, status `200` e o array de produtos.

Compare: a mesma requisição nas duas tecnologias devolve o **mesmo tipo** de resposta.

**O que observar**

- A rota de coleção usa o **plural** e termina com **barra final** (`/api/produtos/`).
- O corpo da resposta é um **array**, pois é uma **coleção**.

---

## 📘 Aula 03 — GET por ID

**O que vamos aprender**

Buscar um **único produto** pelo `id`, usando **parâmetro de rota**, e tratar o caso de não existência
com **`404`**.

**Antes de programar**

Além de listar tudo, queremos pedir **um** produto: `GET /api/produtos/1/`. O `1` é um **parâmetro
de rota** (parte dinâmica da URL). Se o produto não existir, o HTTP tem um status para isso:
**`404 Not Found`**.

**Express**

- **Arquivo:** `express-bsi4/aula3_get_por_id.js`

```js
app.get('/api/produtos/:id/', (req, res) => {
  const produto = produtos.find(p => p.id === parseInt(req.params.id));
  if (!produto) return res.status(404).json({ detail: "Produto não encontrado." });
  res.json(produto);
});
```

- `:id` na rota vira um parâmetro acessível em `req.params.id`.
- `parseInt(...)` converte o valor da URL (string) em número.
- Se não achar, `res.status(404).json({ detail: ... })`.
- Aqui também os dados são os **5 produtos em memória**.

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula3_get_por_id.py`

```python
@app.get("/api/produtos/{id}/")
def buscar_produto_por_id(id: int):
    for produto in produtos:
        if produto["id"] == id:
            return produto
    raise HTTPException(status_code=404, detail="Produto não encontrado.")
```

- `{id}` na URL vira o parâmetro tipado `id: int` da função.
- O FastAPI converte o valor para `int` automaticamente (você declara o tipo).
- Para inexistente, `HTTPException(status_code=404, ...)`.

**Express × FastAPI**

| Conceito         | Express                    | FastAPI                         |
| ---------------- | -------------------------- | ------------------------------- |
| Parâmetro na rota| `:id` e `req.params.id`    | `{id}` e parâmetro `id`         |
| Conversão de tipo| manual (`parseInt`)        | automática via `id: int`        |
| Erro 404         | `res.status(404).json(detail)` | `raise HTTPException(404, detail=...)` |

**Igual:** o contrato `GET /api/produtos/{id}/` → `404` quando não existe, com `detail`.

**Diferente:** no Express o tipo é convertido **manualmente**; no FastAPI o tipo é **declarado** e o
framework converte.

> A operação é a mesma (leitura de um recurso por ID). A declaração é que muda.

**Contrato HTTP**

| Requisição | URL                    | Sucesso | Não encontrado     |
| ---------- | -----------------------| ------- | ------------------ |
| GET        | `/api/produtos/{id}/`   | 200 + produto | 404 + `detail` |

**Pratique no Bruno**

> Abra a coleção (Express e FastAPI), ambiente `Local`. Na pasta **Aula 03**, execute:
> **01 – listar todos**, **02 – buscar por id existente** (200) e **03 – buscar por id inexistente**
> (404). Observe o id na URL e a resposta em cada caso.

**O que observar**

- **Parâmetro de rota** (`/produtos/1`) é diferente de **query param** (`/produtos?search=...`):
  a rota identifica um **recurso**; o query faz **filtros/busca** (veremos na Aula 8).
- O status muda de `200` para `404` quando o recurso não existe.

---

# 🧭 Parte 2 — CRUD

---

## 📘 Aula 04 — POST (criar)

**O que vamos aprender**

Enviar o **corpo** da requisição para **criar** um novo produto (`POST`), com status **`201 Create`**.

**Antes de programar**

Agora o cliente envia dados ao servidor (**corpo da requisição**): `nome` e `preco`. O servidor cria o
produto, gera um novo `id` e devolve o produto com status **`201 Create`**.

**Express**

- **Arquivo:** `express-bsi4/aula4_post.js`

```js
app.post('/api/produtos/', (req, res) => {
  const { nome, preco } = req.body;
  const novoId = produtos.length ? Math.max(...produtos.map(p => p.id)) + 1 : 1;
  const novoProduto = { id: novoId, nome, preco };
  produtos.push(novoProduto);
  res.status(201).json(novoProduto);
});
```

- `app.use(express.json())` é necessário para o Express ler o corpo JSON em `req.body`.
- `req.body` traz os dados enviados.
- Gera um `id` novo com base no maior id existente.
- `res.status(201).json(...)` devolve o produto com status **201**.

**Executar:** `node aula4_post.js` — porta `3000`.

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula4_post.py`

```python
class ProdutoInput(BaseModel):
    nome: str
    preco: float

@app.post("/api/produtos/", status_code=201)
def criar_produto(produto: ProdutoInput):
    novo_id = max(item["id"] for item in produtos) + 1
    novo_produto = {"id": novo_id, "nome": produto.nome, "preco": produto.preco}
    produtos.append(novo_produto)
    return novo_produto
```

- `ProdutoInput(BaseModel)` é o **modelo Pydantic** que representa o corpo esperado.
- O parâmetro tipado `produto: ProdutoInput` — o FastAPI lê e **valida** estruturas do corpo.
- `status_code=201` fixa o status de sucesso.

**Express × FastAPI**

| Conceito       | Express                         | FastAPI                      |
| -------------- | ------------------------------- | ---------------------------- |
| Corpo          | `req.body` (com `express.json()`) | parâmetro tipado (`ProdutoInput`) |
| Modelo do corpo| não há (objeto bruto)           | Pydantic `BaseModel`         |
| Status         | `res.status(201).json(...)`     | `status_code=201`          |

**Igual:** o cliente envia o mesmo JSON e recebe o produto criado com um `id`.

**Diferente:** no Express o corpo é **lido manualmente**; no FastAPI ele é **declarado** como parser
**tipado** e **validado**. A estrutura do corpo já gera **documentação** em `/docs`.

**Contrato HTTP**

| Requisição | URL                  | Corpo                    | Sucesso | Formato |
| ---------- | -------------------- | ----------------------- | ------- | ------- |
| POST       | `/api/produtos/`      | `{"nome": "...", "preco": ...}` | 201 + produto criado | produto criado em JSON |

**Pratique no Bruno**

> Na pasta **Aula 04**, execute **01 – listar todos** (antes), **02 – criar produto** (com o corpo
> JSON no POST, `201`) e **03 – buscar criado** (200). Observe que o corpo é **enviado** no POST.

**O que observar**

- GET **não** leva corpo; POST **leva**. Isso distingue "ler" e "criar".
- O `201` é para **criação**; o `200` para leitura/atualização.
- O id é gerado pelo servidor — o cliente não precisa escolher.

---

## 📘 Aula 05 — PUT (atualizar)

**O que vamos aprender**

**Atualizar por completo** um produto existente com `PUT /api/produtos/{id}/`.

**Antes de programar**

Depois de criar, é preciso **alterar**. O `PUT` envia os dados no corpo e substitui o recurso naquele
id. Como o `PUT` é **atualização completa**, o corpo deve trazer **todos** os campos (`nome` e
`preco`). Se o id não existir, `404`.

**Express**

- **Arquivo:** `express-bsi4/aula5_put.js`

```js
app.put('/api/produtos/:id/', (req, res) => {
  const index = produtos.findIndex(p => p.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ detail: "Produto não encontrado." });
  const { nome, preco } = req.body;
  produtos[index] = { id: parseInt(req.params.id), nome, preco };
  res.json(produtos[index]);
});
```

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula5_put.py`

```python
@app.put("/api/produtos/{id}/")
def atualizar_produto(id: int, produto: ProdutoInput):
    for index, item in enumerate(produtos):
        if item["id"] == id:
            produtos[index] = {"id": id, "nome": produto.nome, "preco": produto.preco}
            return produtos[index]
    raise HTTPException(status_code=404, detail="Produto não encontrado.")
```

**Express × FastAPI**

| Conceito         | Express                    | FastAPI                         |
| ---------------- | -------------------------- | ------------------------------- |
| Parâmetro na rota| `:id` + `req.params.id`    | `{id}` + `id: int`              |
| Corpo            | `req.body`                 | parâmetro tipado `ProdutoInput` |
| Localizar        | `findIndex(...)`           | `enumerate(produtos)`           |
| Não encontrado   | `res.status(404).json(detail)` | `raise HTTPException(404, detail=...)` |

**Igual:** o contrato é `PUT /api/produtos/{id}/` com o corpo completo no JSON e `404` quando o id
não existe.

**Diferente:** o Express localiza e substitui **manualmente** (índice no array); o FastAPI percorre
com `enumerate` e substitui de forma semelhante, mas com **tipos declarados** na assinatura.

**Por que diferente?** São as abstrações de cada framework: um expõe `req`/`res`; o outro usa
assinatura de função tipada. Em ambos, a **responsabilidade do HTTP** é a mesma: rota, método, corpo,
status.

> O `PUT` é **atualização completa**: o corpo deve trazer `nome` e `preco`.

**Contrato HTTP**

| Requisição | Método | URL                   | Corpo | Sucesso | Inexistente |
| ---------- | ------ | --------------------- | ----- | ------- | ----------- |
| PUT        | PUT    | `/api/produtos/{id}/` | `nome`, `preco` | 200 + atualizado | 404 + `detail` |

**Pratique no Bruno**

> Pasta **Aula 05**: execute **01 – listar**, **02 – atualizar produto** (corpo novo, `200`),
> **03 – conferir atualização** e **04 – atualizar id inexistente** (`404`).

**O que observar**

- O `PUT` é **idempotente**: repetir o mesmo `PUT` produz o mesmo resultado.
- Criar (`POST`) e atualizar (`PUT`): o segundo **exige um id** na rota.

---

## 📘 Aula 06 — DELETE (remover)

**O que vamos aprender**

Remover um produto com `DELETE /api/produtos/{id}/`, retornando **`204 No Content`** (sucesso sem
corpo).

**Antes de programar**

O `DELETE` não precisa de corpo. Após excluir, o comum é devolver **`204 No Content`** (sem corpo) ou
**`404`** se o recurso não existir.

**Express**

- **Arquivo:** `express-bsi4/aula6_delete.js`

```js
app.delete('/api/produtos/:id/', (req, res) => {
  const index = produtos.findIndex(p => p.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ detail: "Produto não encontrado." });
  produtos.splice(index, 1);
  res.status(204).end();
});
```

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula6_delete.py`

```python
@app.delete("/api/produtos/{id}/", status_code=204)
def remover_produto(id: int):
    for index, item in enumerate(produtos):
        if item["id"] == id:
            produtos.pop(index)
            return
    raise HTTPException(status_code=404, detail="Produto não encontrado.")
```

**Express × FastAPI**

| Conceito        | Express                    | FastAPI                         |
| --------------- | -------------------------- | ------------------------------- |
| Remover         | `splice(index, 1)`         | `pop(index)`                    |
| Resposta 204    | `res.status(204).end()`    | `status_code=204` na rota + `return` |
| Não encontrado  | `res.status(404).json(detail)` | `raise HTTPException(404, detail=...)` |

**Igual:** o contrato é `DELETE /api/produtos/{id}/` → `204` (sem corpo) quando remove e `404`
quando o id não existe.

**Diferente:** o `204` exige um cuidado de cada framework — no Express o status é enviado e a
resposta **encerrada sem corpo**; no FastAPI o `status_code=204` é declarado na rota e a função
**não devolve corpo**.

**Por que diferente?** O Express controla a resposta via `res`; o FastAPI declara o status na rota. A
**responsabilidade do HTTP** é a mesma: `204 No Content` significa "deu certo, sem corpo".

**Contrato HTTP**

| Requisição | Método | URL                   | Sucesso              | Inexistente |
| ---------- | ------ | --------------------- | -------------------- | ----------- |
| DELETE     | DELETE | `/api/produtos/{id}/` | 204 (sem corpo)      | 404 + `detail` |

**Pratique no Bruno**

> Pasta **Aula 06**: **01 – listar**, **02 – remover produto** (`204`), **03 – buscar removido**
> (`404`), **04 – remover de novo** (também `404`).

**O que observar**

- `204` **não tem corpo** (não é `200` com corpo).
- Excluir é **idempotente**: após remover, uma nova chamada ao mesmo id devolve `404`.

---

# 🧭 Parte3 — Refinando a API

---

## 📘 Aula 07 — Validação

**O que vamos aprender**

**Validar** os dados recebidos para que a API não aceite valores inválidos, devolvendo **`400`** com
`detail`.

**Antes de programar**

Até aqui, um `POST` com dados inconsistentes criava um produto. Vamos aplicar regras:

- `nome`: obrigatório, string, com `trim`, entre **2 e 100** caracteres.
- `preco`: obrigatório, numérico, **> 0**, no máximo **2 casas decimais**.

Erros devolvidos com **`400 Bad Request`** e `detail` por campo.

**Express**

- **Arquivo:** `express-bsi4/aula7_validacao.js`

`validarProduto({ nome, preco })` devolve um objeto de erros; vazio = válido.

```js
function validarProduto({ nome, preco }) {
  const erros = {};
  if (nome === undefined) erros.nome = "O campo é obrigatório.";
  else if (typeof nome !== "string") erros.nome = "O campo deve ser uma string.";
  else {
    const n = nome.trim();
    if (n === "") erros.nome = "O campo não pode ser vazio.";
    else if (n.length < 2 || n.length > 100) erros.nome = "O nome deve possuir entre 2 e 100 caracteres.";
  }
  // ... validações de preco ...
  return erros;
}
```

No POST e no PUT, se houver erros: `res.status(400).json({ detail: erros })`.

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula7_validacao.py`

Aqui a validação também é **explícita** (igual ao Express), para facilitar a comparação.

```python
def validar_produto(nome, preco):
    erros = {}
    if nome is None:
        erros["nome"] = "O campo é obrigatório."
    elif not isinstance(nome, str):
        erros["nome"] = "O campo deve ser uma string."
    else:
        n = nome.strip()
        if n == "":
            erros["nome"] = "O campo não pode ser vazio."
        elif len(n) < 2 or len(n) > 100:
            erros["nome"] = "O nome deve possuir entre 2 e 100 caracteres."
    # ... preco ...
    return erros
```

No POST/PUT, se `erros` não estiver vazio: `raise HTTPException(400, detail=erros)`.

**Express × FastAPI**

| Conceito     | Express        | FastAPI             |
| ------------ | -------------- | ------------------- |
| Validação    | função manual `validarProduto` | função manual `validar_produto` |
| Erro 400     | `res.status(400).json({ detail })` | `raise HTTPException(400, detail=...)` |

**Atenção didática:** nesta aula os dois fazem a validação **manualmente**, com lógica semelhante. A
diferença de implementação está mais em **como retornam o `400`**. (O Pydantic, usado pelo FastAPI,
tem recursos mais "declarativos", mas mantivemos a versão explícita para visualizar a comparação.)

> Mesmo com implementações parecidas, o **contrato** é o mesmo: `400` + `detail` por campo.

**Contrato HTTP**

| Caso | Status | Resposta |
| ---- | ------ | -------- |
| Dados válidos        | `201` / `200` | produto |
| `nome` inválido      | `400` | `detail: { "nome": "..." }` |
| `preco` inválido     | `400` | `detail: { "preco": "..." }` |

**Pratique no Bruno**

> Pasta **Aula 07** — teste casos de erro: **02 nome vazio**, **03 nome curto**, **04 preco zero**,
> **05 preco com 3 casas**, e casos de sucesso (**06 put inválido**, **07 post válido**). Observe o
> formato `detail` e o status `400`.

---

## 📘 Aula 08 — Filtros

**O que vamos aprender**

Filtrar a coleção por faixa de preço com **query params** (`preco_minimo`, `preco_maximo`).

**Antes de programar**

Queremos `GET /api/produtos/?preco_minimo=100&preco_maximo=1000` devolvendo só os produtos entre
`100` e `1000`. Os valores chegam através da **query string**.

**Express**

- **Arquivo:** `express-bsi4/aula8_filtros.js`

```js
const { preco_minimo, preco_maximo } = req.query;
let resultado = [...produtos];              // cópia
if (preco_minimo !== undefined && preco_minimo !== "")
  resultado = resultado.filter(p => p.preco >= Number(preco_minimo));
if (preco_maximo !== undefined && preco_maximo !== "")
  resultado = resultado.filter(p => p.preco <= Number(preco_maximo));
res.json(resultado);
```

- `req.query` traz os query params.
- `filter()` devolve uma nova lista com os que atendem, sobre uma **cópia**.

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula8_filtros.py`

```python
from fastapi import HTTPException


@app.get("/api/produtos/")
def listar_produtos(preco_minimo: str | None = None, preco_maximo: str | None = None):
  resultado = produtos
  try:
    if preco_minimo is not None:
      valor_minimo = float(preco_minimo)
      resultado = [p for p in resultado if p["preco"] >= valor_minimo]
    if preco_maximo is not None:
      valor_maximo = float(preco_maximo)
      resultado = [p for p in resultado if p["preco"] <= valor_maximo]
  except ValueError:
    raise HTTPException(status_code=400, detail="O preço deve ser numérico.")
  return resultado
```

- Os parâmetros da função **viram query params** automaticamente.
- `preco_minimo: str | None` é `None` quando ausente; quando informado, seu texto é convertido para `float` antes da comparação.
- Se a conversão falhar, a API responde `400` com `detail`.

**Express × FastAPI**

| Conceito    | Express                  | FastAPI                  |
| ----------- | ------------------------ | ------------------------ |
| Leitura     | `req.query.preco_minimo` | parâmetro `preco_minimo` |
| Filtro      | `.filter(p => ...)`      | list comprehension        |
| Ausência    | `undefined` / `""`       | `None`                    |

**Contrato:** o que o cliente envia (`?preco_minimo=...`, `?preco_maximo=...`) e a resposta (array
filtrada) são os mesmos.

**Contrato HTTP**

| Query params          | Comportamento        |
| --------------------- | -------------------- |
| `preco_minimo=100`    | apenas `preco >= 100`|
| `preco_maximo=1000`   | apenas `preco <= 1000`|
| valor não numérico    | `400` + `detail`     |

**Pratique no Bruno**

> Pasta **Aula 08**: **01 sem filtro**, **02 preco_minimo**, **03 preco_maximo**, **04 intervalo**,
> **05 valor inválido** (400). Varie os valores e veja como a resposta muda.

**O que observar**

- **Query param** (filtro) vs **parâmetro de rota** (identidade): o filtro não identifica um
  recurso; apenas afina a lista.
- O filtro não altera os dados; **seleciona** um subconjunto.

---

## 📘 Aula 09 — Busca

**O que vamos aprender**

Fazer **busca textual** por nome com `search`, de forma **parcial** e **case-insensitive**.

**Antes de programar**

`GET /api/produtos/?search=mouse` deve devolver produtos cujo nome **contenha** "mouse" (ex. "Mouse
USB"). Busca **parcial** (qualquer parte do nome) e sem diferenciar maiúsculas.

**Express**

- **Arquivo:** `express-bsi4/aula9_busca.js`

```js
if (search !== undefined && search !== "") {
  const termo = search.toLowerCase();
  resultado = resultado.filter(p => p.nome.toLowerCase().includes(termo));
}
```

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula9_busca.py`

```python
if search is not None:
    termo = search.lower()
    resultado = [p for p in resultado if termo in p["nome"].lower()]
```

**Express × FastAPI**

| Conceito            | Express                  | FastAPI                |
| ------------------- | ------------------------ | ---------------------- |
| Verificar termo     | `includes()`             | `in`                   |
| Ignorar maiúsculas  | `toLowerCase()`          | `.lower()`             |

**Igual:** a lógica é a mesma — baixar a caixa dos dois lados e verificar se o termo está contido no
nome. **Diferente:** a sintaxe (método JS vs operador Python). **Por que diferente?** a linguagem;
**responsabilidade do HTTP:** nenhuma, a busca é inteiramente do framework (o parâmetro `search` é só
um query param).

**Contrato HTTP**

| Parâmetro | Comportamento |
| --------- | ------------------------- |
| `search=mouse` | produtos com "mouse" no nome |

**Pratique no Bruno**

> Pasta **Aula 09**: **01 busca mouse**, **02 busca case-insensitive**, **03 busca tecla** (termo
> parcial), **04 sem resultado** (lista vazia).

**O que observar**

- Busca **parcial**: "tec" retorna "Teclado USB".
- **Case-insensitive**: `MOUSE` e `mouse` dão o mesmo resultado.

---

## 📘 Aula 10 — Ordenação

**O que vamos aprender**

Ordenar os resultados com `ordering`, que aceita `nome` ou `preco`, com o prefixo `-` para
decrescente.

**Antes de programar**

`GET /api/produtos/?ordering=nome` (crescente) e `?ordering=-preco` (decrescente). Precisamos aceitar
o prefixo `-` e validar o campo informado.

**Express**

- **Arquivo:** `express-bsi4/aula10_ordenacao.js`

```js
const camposOrdenacao = ["nome", "preco"];
let campoOrdenacao = null;
let ordemDesc = false;
if (ordering !== undefined && ordering !== "") {
  const valor = ordering.startsWith("-") ? ordering.slice(1) : ordering;
  const desc = ordering.startsWith("-");
  if (!camposOrdenacao.includes(valor)) {
    erros.ordering = "Campo de ordenação inválido.";
  } else {
    campoOrdenacao = valor;
    ordemDesc = desc;
  }
}
if (campoOrdenacao) {
  resultado.sort((a, b) => {
    const comparacao =
      campoOrdenacao === "preco"
        ? a.preco - b.preco
        : a.nome.toLowerCase().localeCompare(b.nome.toLowerCase());
    return ordemDesc ? -comparacao : comparacao;
  });
}
```
(No código real, o Express separa o `-`, valida o campo e ordena a **cópia**.)

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula10_ordenacao.py`

```python
campos_ordenacao = ["nome", "preco"]
campo_ordenacao = None
ordem_desc = False
if ordering is not None:
    valor = ordering[1:] if ordering.startswith("-") else ordering
    if valor in campos_ordenacao:
        campo_ordenacao = valor
        ordem_desc = ordering.startswith("-")
    else:
        erros["ordering"] = "Campo de ordenação inválido."

if campo_ordenacao == "preco":
    resultado.sort(key=lambda p: p["preco"], reverse=ordem_desc)
elif campo_ordenacao == "nome":
    resultado.sort(key=lambda p: p["nome"].lower(), reverse=ordem_desc)
```

**Express × FastAPI**

| Conceito     | Express          | FastAPI                  |
| ------------ | ---------------- | ------------------------ |
| Separar `-`  | `startsWith("-")`| `startswith("-")`        |
| Ordenar num. | `sort((a,b)=>...)` | `sort(key=lambda)`   |
| Decrescente  | inverso          | `reverse=True` (na chave)  |

A **ideia** é a mesma: extrair o campo, validar, ordenar e tratar o `-` como decrescente.

**Contrato HTTP**

| `ordering`   | sentido |
| ------------ | ------- |
| `preco`      | crescente |
| `-preco`     | decrescente |
| `nome`       | alfabético (A→Z) |
| campo inválido | `400` + `detail` |

**Pratique no Bruno**

> **Aula 10**: **01 sem ordenação**, **02 `ordering=nome`**, **03 `ordering=-nome`**, **04
> `ordering=preco`**, **05 campo inválido** (`400`).

**O que observar**

- A ordenação muda a **ordem** dos resultados, mas **ainda não há paginação**.
- O prefixo `-` segue o padrão usado também no Django REST Framework.

---

# 🧭 Parte4 — Persistência e paginação

---

## 📘 Aula 11 — Persistência em JSON

**O que vamos aprender**

**Persistir** a API em um arquivo JSON (`produtos.json`), de modo que os dados **sobrevivam** ao
reinício. Nesta aula passamos dos **5 produtos em memória** para o **dataset persistido de 60
produtos** (ids 1–60). **Ainda não há paginação.**

**Antes de programar**

Até a Aula 10, ao reiniciar o servidor voltavam os 5 produtos fixos. Agora, queremos **guardar em
arquivo** as alterações. A lógica é: **primeiro aprendemos a manipular recursos; depois aprendemos a
persistir**.

> Nesta aula, `GET` devolve um **array simples** (sem paginação). A paginação virá na Aula 12.

**Express**

- **Arquivo:** `express-bsi4/aula11_persistencia_json.js` (usa `fs` e `path`)

```js
const ARQUIVO = path.join(__dirname, 'produtos.json');
function carregarProdutos() { /* lê e faz parse, ou [] */ }
function salvarProdutos(lista) { fs.writeFileSync(ARQUIVO, JSON.stringify(lista, null, 2), 'utf-8'); }
```

- `POST`, `PUT` e `DELETE` **salvam** o arquivo após a alteração; `GET` não escreve.

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula11_persistencia_json.py`

```python
import json

def carregar_produtos():   # lê produtos.json
def salvar_produtos(lista):  # json.dump(...)
```

Mesma ideia: ler no início e salvar após cada alteração.

**Express × FastAPI**

| Conceito | Express                     | FastAPI               |
| -------- | --------------------------- | --------------------- |
| Leitura  | `fs.readFileSync`           | `open` + `json.load`  |
| Escrita  | `fs.writeFileSync`          | `open` + `json.dump`  |
| Módulo   | integrado do Node (`fs`)    | integrado do Python (`json`) |

**Contrato:** igual ao anterior; a diferença é que os dados agora **persistem**. Os **60 produtos**
vêm de `produtos.json`.

**Contrato HTTP**

| Dados | Aulas 2–10 | Aula 11+ |
| ------ | ---------- | -------- |
| Fonte  | 5 em memória | `produtos.json` (60, ids 1–60) |
| Persiste| não        | sim (POST/PUT/DELETE gravam) |

**Pratique no Bruno**

> Pasta **Aula 11** (as duas). Observe que agora há **60 produtos** (**01 – listar do arquivo**), e
> que criar/atualizar/remover **grava** no arquivo (02–07). Reinicie o servidor para confirmar que os
> dados continuam.

**O que observar**

- Diferença entre **array em memória** (perde-se ao reiniciar) e **arquivo persistente** (permanece).
- `produtos.json` é versionado junto do código.

---

## 📘 Aula 12 — Paginação

**O que vamos aprender**

**Paginção** da lista com `page` e `page_size`, resposta `{ page, page_size, total_pages, results }`,
funcionando **junto com filtros, busca e ordenação**.

**Antes de programar**

Com **60 produtos**, devolver todos de uma vez não é legal. A paginação limita a quantidade por
resposta e informa quantas páginas existem. Padrão: `page_size = 10`, máximo `100`.

```json
{ "page": 1, "page_size": 10, "total_pages": 6, "results": [...] }
```

**Express**

- **Arquivo:** `express-bsi4/aula12_paginacao.js`

```js
const totalPages = Math.ceil(totalParaPaginacao / tamanhoPagina);
const inicio = (pagina - 1) * tamanhoPagina;
const itens = resultado.slice(inicio, inicio + tamanhoPagina);
res.json({ page: pagina, page_size: tamanhoPagina, total_pages: totalPages, results: itens });
```

- Valida `page` e `page_size` (inteiro positivo, ≤100).

**FastAPI**

- **Arquivo:** `fastapi-bsi4/aula12_paginacao.py` (usa o modelo `RespostaPaginada`)

```python
total_pages = (total + tamanho_pagina - 1) // tamanho_pagina
inicio = (pagina - 1) * tamanho_pagina
itens = resultado[inicio : inicio + tamanho_pagina]
return RespostaPaginada(page=pagina, page_size=tamanho_pagina, total_pages=total_pages, results=itens)
```

**Express × FastAPI**

| Conceito         | Express                  | FastAPI                        |
| ---------------- | ------------------------ | ------------------------------ |
| `total_pages`    | `Math.ceil(total / tamanho)` | `(total + tamanho - 1) // tamanho` |
| Resposta paginada| objeto direto            | modelo tipado `RespostaPaginada` |

**Igual:** o cálculo de `total_pages` é **equivalente** (a expressão `(total + tamanho - 1) //
tamanho` é a versão "inteira" de `Math.ceil`), e o contrato é o mesmo: `page`, `page_size`,
`total_pages`, `results`.

**Diferente:** o FastAPI usa um **modelo Pydantic** (`RespostaPaginada`) para dar forma tipada; o
Express monta um **objeto direto**. **Por que diferente?** tipagem declarativa vs JS sem tipagem.

> A paginação acontece **depois** do filtro → busca → ordenação (a ordem importa: paginar antes
> traria itens errados).

**Contrato HTTP**

| Parâmetro | Padrão | Descrição |
| --------- | ------ | ---------- |
| `page`    | 1      | número da página (base 1) |
| `page_size` | 10   | itens por página (máx 100) |
| `resultado`|        | `page`, `page_size`, `total_pages`, `results` |

Combinação possível: `?search=mouse&ordering=-preco&page=2&page_size=10`. A ordem é: filtro → busca →
ordenação → paginação.

**Pratique no Bruno**

> Pasta **Aula 12**: teste **01 padrão**, **02 página 2**, **03 última página**, **04 além do
> limite** (results vazia), **05 `page=0`**, **06 `page_size=0`**, **07 `page_size` grande** (400) e
> combinações com filtro/busca/ordenação (08, 09, 10).

**O que observar**

- É preciso aplicar a **ordenação antes** da paginação; senão as páginas ficam erradas.

---

# 🧭 Parte5 — Consolidação

---

## 📘 Aula 13 — API completa

**O que vamos aprender**

Consolidar **todas** as funcionalidades em uma única versão, **sem introduzir conceito novo**.

**O que é a API completa**

- **Arquivos:** `express-bsi4/aula13_api_completa.js` e `fastapi-bsi4/aula13_api_completa.py`.
- Reúne: CRUD + validação + filtros + busca + ordenação + persistência + paginação.
- É o resultado das Aulas 2–12.

**Express × FastAPI — quadro final**

| Aspecto       | Express                       | FastAPI                      |
| ------------- | ----------------------------- | ---------------------------- |
| Linguagem     | JavaScript (Node.js)          | Python                       |
| Framework     | Express                       | FastAPI                      |
| Rotas         | `app.get/post/put/delete(...)`| `@app.get/post/put/delete(...)` |
| Entrada       | `req.body`/`req.params`/`req.query` | parâmetros tipados da função |
| Validação     | função manual `validarProduto`  | função manual `validar_produto` |
| Persistência  | `fs`/`path` + `produtos.json`  | `json` + `produtos.json`      |
| Filtros       | `.filter(...)`                | list comprehension           |
| Busca         | `toLowerCase().includes()`     | `termo in nome.lower()`       |
| Ordenação     | `sort((a,b)=>...)`             | `sort(key=..., reverse=...)`  |
| Paginação     | `slice` + `Math.ceil`           | slicing + `RespostaPaginada` |
| Documentação  | nenhuma automática (manual/Bruno) | `/docs` automática (Swagger/OpenAPI) |

> A documentação automática (`/docs`) é um benefício do FastAPI, mas **não altera o contrato**: a
> API consumida é a mesma nos dois frameworks.

**Contrato HTTP**

Documentação do contrato que Express e FastAPI atendem:

| Operação      | Método | Endpoint                | Status principal | Resposta                       |
| ------------- | ------ | ----------------------- | ---------------- | ------------------------------ |
| Listar        | GET    | `/api/produtos/`          | 200              | coleção de produtos            |
| Buscar por ID | GET    | `/api/produtos/{id}/`     | 200 / 404        | produto individual / `detail`  |
| Criar         | POST   | `/api/produtos/`          | 201              | produto criado                 |
| Atualizar     | PUT    | `/api/produtos/{id}/`     | 200 / 404        | produto atualizado / `detail`  |
| Excluir       | DELETE | `/api/produtos/{id}/`     | 204 / 404        | sem corpo (204) / `detail`     |

- **Validação:** `nome` entre 2 e 100 caracteres (com `trim`); `preco` numérico maior que zero, com
  no máximo 2 casas decimais.
- **Filtros:** `preco_minimo`, `preco_maximo` (ex.: `/api/produtos/?preco_minimo=100`).
- **Busca:** `search` (ex.: `/api/produtos/?search=mouse`).
- **Ordenação:** `ordering=nome` / `-nome` / `preco` / `-preco`.
- **Paginação:** `page` (padrão 1) e `page_size` (padrão 10, máximo 100); resposta com
  `page`, `page_size`, `total_pages` e `results`.
- **Formato de erro:** `{ "detail": ... }`.

> Esse é o contrato que qualquer cliente consumidor recebe — identificado em cada framework conforme
> evoluímos nas Aulas 2–12.

**Pratique no Bruno**

A pasta **Aula 13 – Integração** exercita o fluxo completo: listar, criar, ler, atualizar, remover,
confirmar remoção e um caso inválido. Execute **tudo** nas duas tecnologias e observe o ciclo de vida
de um recurso.

**Conclusão**

O objetivo não era aprender duas sintaxes para fazer a mesma coisa. Era **compreender o contrato de
uma API HTTP** e perceber como **diferentes frameworks implementam esse contrato**. Ao final, você
vê a **mesma API** construída de duas formas.

```text
endpoint simples → recurso por ID → CRUD → validação → filtros → busca → ordenação
→ persistência → paginação → API completa
```

> **O framework muda a forma de implementar. O contrato HTTP permanece.**

---

# 🧭 Parte 6 — Exercícios — novos campos na entidade

> **Objetivo:**

> Até a Aula 13, construímos uma API completa para um modelo simples: `Produto(id, nome, preco)`.
> Agora, vamos vivenciar na prática o que acontece quando o modelo de negócio evolui e novos campos precisam ser adicionados.
>
> Adicionar um campo a uma entidade **não é apenas alterar a estrutura dos dados (JSON)**. Cada novo atributo pode exigir mudanças em:
> - **Criação e atualização (POST e PUT):** receber, processar e persistir o novo dado;
> - **Validação:** garantir regras de tipo, formato, obrigatoriedade e limites;
> - **Filtros:** permitir que o cliente filtre a coleção por esse campo;
> - **Ordenação:** permitir que a listagem seja ordenada crescente ou decrescentemente por esse campo;
> - **Busca textual:** decidir se o novo campo deve ser incluído na busca global (`search`);
> - **Testes e documentação:** atualizar as coleções de requisições e garantir que todos os cenários (válidos e inválidos) continuem funcionando.

Esta seção é **incremental e cumulativa**: cada aula adiciona um novo campo sobre o código da aula anterior. Inclusive o arquivo com os produtos (produtos.json) terá sua estrutura alterada a cada aula e não funcionará com os arquivos das aulas anteriores. Se vocẽ quiser que ele funcione com as aulas anteriores, faça uma cópia do arquivo products.json para cada aula e utilize outro nome para o arquivo. Por exemplo, na aula 14, utilize o arquivo products14.json.

> **💡 Como realizar estes exercícios (Dinâmica Ativa):**
> 1. **Escolha da tecnologia:** Você pode optar por implementar os exercícios em apenas **uma** das tecnologias (**Express** ou **FastAPI**). Se preferir e quiser aprofundar a comparação, sinta-se à vontade para fazer nas **duas**!
> 2. **Leia a especificação e as regras de negócio** de cada tópico com atenção antes de olhar o código.
> 3. **Tente programar a alteração sozinho** no seu arquivo da aula anterior (`aula13_api_completa` ou uma cópia dedicada para o exercício).
> 4. **Abra o bloco retrátil** apenas para **conferir sua solução**, comparar abordagens ou tirar dúvidas caso trave.
> 5. **Execute e valide no Bruno** usando a tabela de testes fornecida ao final de cada aula.

---

## 📘 Aula 14 — Adicionando marca

**1. O que vamos aprender e o problema**

Nesta aula adicionamos o campo **`marca`** à entidade `Produto`.

Ao introduzir `marca`, o produto passa a ter 4 atributos: `id`, `nome`, `preco` e `marca`. Nosso objetivo é integrar esse campo a todas as operações já existentes na API, garantindo que o cliente possa criar, atualizar, validar, filtrar, ordenar e pesquisar produtos pela marca.

**2. Alteração do modelo e dados**

O objeto de produto passa a ter a estrutura:

```json
{
  "id": 1,
  "nome": "Notebook Pro",
  "preco": 3500.0,
  "marca": "Dell"
}
```

- No **Express**, o `req.body` em POST e PUT passa a extrair `marca`, e o objeto persistido inclui `marca: marca.trim()`.
- No **FastAPI**, o modelo `ProdutoInput` deve ser atualizado com o novo campo.

<details>
<summary><strong>Ver alteração no req.body e persistência no Express</strong></summary>

```js
const { nome, preco, marca } = req.body;

const erros = validarProduto({ nome, preco, marca });
if (Object.keys(erros).length > 0) {
  return res.status(400).json({ detail: erros });
}

const novoProduto = {
  id: novoId,
  nome: nome.trim(),
  preco,
  marca: marca.trim(),
};
```

Em `PUT`, o mesmo padrão vale para o objeto existente: `marca` entra no payload e é persistido já normalizado com `trim()`, sem espaços nas bordas.

</details>

<details>
<summary><strong>Ver alteração no ProdutoInput (FastAPI)</strong></summary>

```python
class ProdutoInput(BaseModel):
    nome: str | None = None
    preco: float | None = None
    marca: str | None = None
```

</details>

**3. Validação**

**Regras para o campo `marca`:**
- **Obrigatório** (não pode ser omitido);
- **Deve ser string**;
- **Não pode ser vazio** (após remover espaços em branco nas pontas);
- **Tamanho:** deve possuir entre **2 e 50 caracteres**.

> ✏️ **Desafio:** Atualize a função de validação manual no seu código para validar o campo `marca` conforme as regras acima e retornar `400 Bad Request` com o formato `{ "detail": { "marca": "..." } }`.

<details>
<summary><strong>Ver solução no Express (Node.js)</strong></summary>

Na função `validarProduto({ nome, preco, marca })`:

```js
// Validação de marca
if (marca === undefined) {
  erros.marca = "O campo é obrigatório.";
} else if (typeof marca !== "string") {
  erros.marca = "O campo deve ser uma string.";
} else {
  const marcaLimpa = marca.trim();
  if (marcaLimpa === "") {
    erros.marca = "O campo não pode ser vazio.";
  } else if (marcaLimpa.length < 2 || marcaLimpa.length > 50) {
    erros.marca = "A marca deve possuir entre 2 e 50 caracteres.";
  }
}
```

</details>

<details>
<summary><strong>Ver solução no FastAPI (Python)</strong></summary>

Na função `validar_produto(nome, preco, marca)`:

```python
# Validação de marca
if marca is None:
    erros["marca"] = "O campo é obrigatório."
elif not isinstance(marca, str):
    erros["marca"] = "O campo deve ser uma string."
else:
    marca_limpa = marca.strip()
    if marca_limpa == "":
        erros["marca"] = "O campo não pode ser vazio."
    elif len(marca_limpa) < 2 or len(marca_limpa) > 50:
        erros["marca"] = "A marca deve possuir entre 2 e 50 caracteres."
```

</details>

**4. Filtro por marca**

Queremos permitir consultas como `GET /api/produtos/?marca=Samsung`. O filtro deve ser exato para o termo, mas insensível a maiúsculas/minúsculas (*case-insensitive*), e deve combinar perfeitamente com `preco_minimo` e `preco_maximo`.

> ✏️ **Desafio:** Receba o query param `marca` e filtre a lista de produtos de forma case-insensitive, mantendo os filtros por preço funcionando em conjunto.

<details>
<summary><strong>Ver solução de filtro no Express</strong></summary>

```js
const { marca, preco_minimo, preco_maximo, ... } = req.query;

if (marca !== undefined && marca !== "") {
  const termoMarca = marca.toLowerCase();
  resultado = resultado.filter(p => p.marca && p.marca.toLowerCase() === termoMarca);
}
```

</details>

<details>
<summary><strong>Ver solução de filtro no FastAPI</strong></summary>

```python
@app.get("/api/produtos/", response_model=RespostaPaginada)
def listar_produtos(
    marca: str | None = None,
    preco_minimo: str | None = None,
    preco_maximo: str | None = None,
    ...
):
    ...
    if marca is not None and marca != "":
        termo_marca = marca.lower()
        resultado = [p for p in resultado if p.get("marca", "").lower() == termo_marca]
```

</details>

**5. Ordenação por marca**

Permitir `ordering=marca` (crescente, A→Z) e `ordering=-marca` (decrescente, Z→A).

> ✏️ **Desafio:** Adicione `"marca"` à lista de campos permitidos na ordenação e implemente a comparação alfabética de strings.

<details>
<summary><strong>Ver solução de ordenação no Express</strong></summary>

```js
const camposOrdenacao = ["nome", "preco", "marca"];
// ...
if (campoOrdenacao === "preco") {
  comparacao = a.preco - b.preco;
} else if (campoOrdenacao === "marca") {
  comparacao = a.marca.toLowerCase().localeCompare(b.marca.toLowerCase());
} else {
  comparacao = a.nome.toLowerCase().localeCompare(b.nome.toLowerCase());
}
```

</details>

<details>
<summary><strong>Ver solução de ordenação no FastAPI</strong></summary>

```python
campos_ordenacao = ["nome", "preco", "marca"]
# ...
if campo_ordenacao == "preco":
    resultado.sort(key=lambda p: p["preco"], reverse=ordem_desc)
elif campo_ordenacao == "marca":
    resultado.sort(key=lambda p: p.get("marca", "").lower(), reverse=ordem_desc)
elif campo_ordenacao == "nome":
    resultado.sort(key=lambda p: p["nome"].lower(), reverse=ordem_desc)
```

</details>

**6. Busca textual (`search`)**

A partir desta aula, `marca` passa a fazer parte da busca textual. O parâmetro `?search=termo` deve encontrar produtos em que o termo apareça no **`nome`** OU na **`marca`**.

> ✏️ **Desafio:** Amplie a expressão lógica da busca textual para pesquisar tanto no `nome` quanto na `marca`.

<details>
<summary><strong>Ver solução de busca textual no Express</strong></summary>

```js
if (search !== undefined && search !== "") {
  const termo = search.toLowerCase();
  resultado = resultado.filter(p =>
    p.nome.toLowerCase().includes(termo) ||
    (p.marca && p.marca.toLowerCase().includes(termo))
  );
}
```

</details>

<details>
<summary><strong>Ver solução de busca textual no FastAPI</strong></summary>

```python
if search is not None:
    termo = search.lower()
    resultado = [
        p for p in resultado
        if termo in p["nome"].lower() or (p.get("marca") and termo in p["marca"].lower())
    ]
```

</details>

**7. Contrato HTTP e testes no Bruno**

| Cenário de Teste | Método | URL | Corpo (JSON) | Status Esperado | O que validar |
| --- | --- | --- | --- | --- | --- |
| **01. Criar produto com marca válida** | POST | `/api/produtos/` | `{"nome": "Monitor Ultra", "preco": 1200.0, "marca": "Dell"}` | `201 Created` | Retorna produto com `id` gerado e `"marca": "Dell"` |
| **02. Criar com marca ausente** | POST | `/api/produtos/` | `{"nome": "Monitor Ultra", "preco": 1200.0}` | `400 Bad Request` | `detail.marca` indica campo obrigatório |
| **03. Criar com marca vazia/curta** | POST | `/api/produtos/` | `{"nome": "Monitor", "preco": 1000.0, "marca": " "}` | `400 Bad Request` | `detail.marca` indica erro de validação |
| **04. Atualizar marca via PUT** | PUT | `/api/produtos/1/` | `{"nome": "Notebook Pro", "preco": 3800.0, "marca": "Lenovo"}` | `200 OK` | Produto atualizado com a nova marca |
| **05. Filtrar por marca existente** | GET | `/api/produtos/?marca=Dell` | — | `200 OK` | Apenas produtos com marca Dell na lista |
| **06. Filtrar por marca inexistente** | GET | `/api/produtos/?marca=MarcaFantasma` | — | `200 OK` | `results` vazia (`[]`), `total_pages: 0` |
| **07. Combinar marca e preço** | GET | `/api/produtos/?marca=Dell&preco_minimo=2000` | — | `200 OK` | Produtos Dell com preço >= 2000 |
| **08. Ordenação crescente por marca** | GET | `/api/produtos/?ordering=marca` | — | `200 OK` | Lista em ordem alfabética de marca (A→Z) |
| **09. Ordenação decrescente por marca** | GET | `/api/produtos/?ordering=-marca` | — | `200 OK` | Lista em ordem reversa de marca (Z→A) |
| **10. Busca textual pela marca** | GET | `/api/produtos/?search=dell` | — | `200 OK` | Encontra produtos cuja marca contenha "dell" |
| **11. Busca sem resultados** | GET | `/api/produtos/?search=termoinexistente` | — | `200 OK` | `results` vazia |

**8. O que observar**

Note a quantidade de partes que precisamos alterar para dar suporte a **um único campo**:
1. O modelo de dados e persistência;
2. As rotas POST e PUT;
3. A função de validação com regras de string e tamanho;
4. Os parâmetros aceitos no GET (query params);
5. A lógica do filtro;
6. A lista de campos aceitos na ordenação e a comparação de strings;
7. A expressão lógica da busca textual;
8. As requisições de teste.

---

## 📘 Aula 15 — Adicionando estoque

**1. O que vamos aprender e o problema**

Nesta aula adicionamos o campo **`estoque`** à entidade `Produto`, partindo do código da Aula 14. O produto agora possui: `id`, `nome`, `preco`, `marca` e `estoque`.

Ao contrário de `nome` e `marca` (que são textos), `estoque` é uma **quantidade inteira**. Isso traz novos desafios para validação e filtragem por faixa de valores. Além disso, veremos por que **nem todo campo deve entrar na busca textual**.

**2. Alteração do modelo e dados**

O objeto de produto passa a ter a estrutura:

```json
{
  "id": 1,
  "nome": "Notebook Pro",
  "preco": 3500.0,
  "marca": "Dell",
  "estoque": 15
}
```

- No **Express**, extraímos `estoque` no POST/PUT e persistimos como número inteiro.
- No **FastAPI**, atualizamos o `ProdutoInput`:

<details>
<summary><strong>Ver alteração no req.body e persistência no Express</strong></summary>

```js
const { nome, preco, marca, estoque } = req.body;

const erros = validarProduto({ nome, preco, marca, estoque });
if (Object.keys(erros).length > 0) {
  return res.status(400).json({ detail: erros });
}

const novoProduto = {
  id: novoId,
  nome: nome.trim(),
  preco,
  marca: marca.trim(),
  estoque: Number(estoque),
};
```

Em `PUT`, o estoque também entra no payload e é salvo como valor numérico inteiro, respeitando a regra `estoque >= 0`.

</details>

<details>
<summary><strong>Ver alteração no ProdutoInput (FastAPI)</strong></summary>

```python
class ProdutoInput(BaseModel):
    nome: str | None = None
    preco: float | None = None
    marca: str | None = None
    estoque: int | None = None
```

</details>

**3. Validação**

**Regras para o campo `estoque`:**
- **Obrigatório**;
- **Deve ser um número inteiro** (não pode ser string, booleano ou float com casas decimais);
- **Não pode ser negativo** (`estoque >= 0`).

Exemplos:
- `estoque = 10` → ✅ válido
- `estoque = 0` → ✅ válido (estoque zerado é comum no comércio)
- `estoque = -1` → ❌ inválido (não existe estoque negativo)
- `estoque = "dez"` → ❌ inválido (tipo incorreto)
- `estoque = 5.5` → ❌ inválido (unidades de estoque são inteiras)

> ✏️ **Desafio:** Implemente a validação de `estoque` na sua função de validação, garantindo que valores negativos, não inteiros ou de tipos incorretos retornem `400 Bad Request` com `{ "detail": { "estoque": "..." } }`.

<details>
<summary><strong>Ver validação de estoque no Express</strong></summary>

Na função `validarProduto`:

```js
// Validação de estoque
if (estoque === undefined) {
  erros.estoque = "O campo é obrigatório.";
} else if (typeof estoque !== "number" || !Number.isInteger(estoque)) {
  erros.estoque = "O campo deve ser um número inteiro.";
} else if (estoque < 0) {
  erros.estoque = "O estoque não pode ser negativo.";
}
```

</details>

<details>
<summary><strong>Ver validação de estoque no FastAPI</strong></summary>

Na função `validar_produto`:

```python
# Validação de estoque
if estoque is None:
    erros["estoque"] = "O campo é obrigatório."
elif not isinstance(estoque, int) or isinstance(estoque, bool):
    erros["estoque"] = "O campo deve ser um número inteiro."
elif estoque < 0:
    erros["estoque"] = "O estoque não pode ser negativo."
```

</details>

**4. Filtros por estoque**

Para trabalhar com quantidades em estoque, implementamos os query params:
- `estoque_minimo`: retorna produtos com `estoque >= estoque_minimo`
- `estoque_maximo`: retorna produtos com `estoque <= estoque_maximo`

Se o usuário fornecer um valor não numérico nesses parâmetros (ex.: `?estoque_minimo=abc`), a API deve devolver **`400 Bad Request`**.

> ✏️ **Desafio:** Implemente a leitura e validação dos query params `estoque_minimo` e `estoque_maximo`, filtrando os produtos pela quantidade em estoque e retornando `400` se os valores informados forem inválidos.

<details>
<summary><strong>Ver filtros de estoque no Express</strong></summary>

```js
const { estoque_minimo, estoque_maximo, ... } = req.query;

if (estoque_minimo !== undefined && estoque_minimo !== "") {
  if (!/^[0-9]+$/.test(estoque_minimo)) {
    erros.estoque_minimo = "O valor deve ser um número inteiro não negativo.";
  } else {
    resultado = resultado.filter(p => p.estoque >= parseInt(estoque_minimo, 10));
  }
}

if (estoque_maximo !== undefined && estoque_maximo !== "") {
  if (!/^[0-9]+$/.test(estoque_maximo)) {
    erros.estoque_maximo = "O valor deve ser um número inteiro não negativo.";
  } else {
    resultado = resultado.filter(p => p.estoque <= parseInt(estoque_maximo, 10));
  }
}
```

</details>

<details>
<summary><strong>Ver filtros de estoque no FastAPI</strong></summary>

```python
@app.get("/api/produtos/", response_model=RespostaPaginada)
def listar_produtos(
    estoque_minimo: str | None = None,
    estoque_maximo: str | None = None,
    ...
):
    ...
    if estoque_minimo is not None:
        if not estoque_minimo.isdigit():
            erros["estoque_minimo"] = "O valor deve ser um número inteiro não negativo."
        else:
            val_min = int(estoque_minimo)
            resultado = [p for p in resultado if p.get("estoque", 0) >= val_min]

    if estoque_maximo is not None:
        if not estoque_maximo.isdigit():
            erros["estoque_maximo"] = "O valor deve ser um número inteiro não negativo."
        else:
            val_max = int(estoque_maximo)
            resultado = [p for p in resultado if p.get("estoque", 0) <= val_max]
```

</details>

**5. Ordenação por estoque**

Permitir `ordering=estoque` (crescente) e `ordering=-estoque` (decrescente).

> ✏️ **Desafio:** Adicione `"estoque"` aos campos permitidos de ordenação e implemente a ordenação numérica crescente e decrescente.

<details>
<summary><strong>Ver ordenação por estoque no Express</strong></summary>

```js
} else if (campoOrdenacao === "estoque") {
  comparacao = a.estoque - b.estoque;
}
```

</details>

<details>
<summary><strong>Ver ordenação por estoque no FastAPI</strong></summary>

```python
elif campo_ordenacao == "estoque":
    resultado.sort(key=lambda p: p.get("estoque", 0), reverse=ordem_desc)
```

</details>

**6. Busca textual: Por que NÃO incluir estoque?**

> [!IMPORTANT]
> **O campo `estoque` NÃO deve ser incluído na busca textual (`search`).**

A busca textual tem como objetivo encontrar itens a partir de **termos textuais e palavras-chave** (nomes, marcas, categorias).

Se incluíssemos o estoque na busca, uma requisição como `GET /api/produtos/?search=10` retornaria produtos que têm "10" no nome, produtos da marca "10" E também qualquer produto que por acaso tivesse exatamente 10 unidades no estoque. Isso poluiria os resultados com correspondências sem sentido para o usuário.

Para consultar quantidades numéricas, utilizamos **filtros dedicados** (`estoque_minimo`, `estoque_maximo`), e não a busca por texto livre.

**7. Contrato HTTP e testes no Bruno**

| Cenário de Teste | Método | URL | Corpo (JSON) | Status Esperado | O que validar |
| --- | --- | --- | --- | --- | --- |
| **01. Criar com estoque válido** | POST | `/api/produtos/` | `{"nome": "Mouse Sem Fio", "preco": 80.0, "marca": "Logitech", "estoque": 25}` | `201 Created` | Retorna produto com `"estoque": 25` |
| **02. Criar com estoque zero** | POST | `/api/produtos/` | `{"nome": "Teclado Mecânico", "preco": 250.0, "marca": "Keychron", "estoque": 0}` | `201 Created` | Sucesso (`estoque: 0` é válido) |
| **03. Criar com estoque negativo** | POST | `/api/produtos/` | `{"nome": "Fone", "preco": 150.0, "marca": "Sony", "estoque": -5}` | `400 Bad Request` | `detail.estoque` avisa que não pode ser negativo |
| **04. Criar com tipo inválido** | POST | `/api/produtos/` | `{"nome": "Fone", "preco": 150.0, "marca": "Sony", "estoque": "muitos"}` | `400 Bad Request` | `detail.estoque` avisa que deve ser inteiro |
| **05. Filtrar por estoque mínimo** | GET | `/api/produtos/?estoque_minimo=10` | — | `200 OK` | Apenas produtos com estoque >= 10 |
| **06. Filtrar por estoque máximo** | GET | `/api/produtos/?estoque_maximo=5` | — | `200 OK` | Apenas produtos com estoque <= 5 (itens acabando) |
| **07. Filtrar por faixa de estoque** | GET | `/api/produtos/?estoque_minimo=10&estoque_maximo=30` | — | `200 OK` | Apenas produtos no intervalo [10, 30] |
| **08. Ordenar crescente por estoque** | GET | `/api/produtos/?ordering=estoque` | — | `200 OK` | Do menor estoque para o maior |
| **09. Ordenar decrescente por estoque** | GET | `/api/produtos/?ordering=-estoque` | — | `200 OK` | Do maior estoque para o menor |
| **10. Combinar marca, preço e estoque** | GET | `/api/produtos/?marca=Dell&preco_minimo=1000&estoque_minimo=1` | — | `200 OK` | Produtos Dell caros que estão disponíveis |

**8. O que observar**

- Atributos numéricos demandam validações de tipo estritas (número vs texto, inteiro vs ponto flutuante, faixas positivas/negativas).
- Filtros de intervalo (`min`/`max`) são a forma idiomática de filtrar grandezas numéricas em APIs REST.
- A modelagem de uma API exige decisões de design conscientes: nem todo campo participa de todos os mecanismos (como a busca textual).

---

## 📘 Aula 16 — Adicionando descrição

**1. O que vamos aprender e o problema**

Nesta aula completamos o ciclo de expansão adicionando o campo **`descricao`**. A entidade `Produto` agora atinge sua estrutura final nesta fase:
`id`, `nome`, `preco`, `marca`, `estoque` e `descricao`.

Diferente de `nome` e `marca` (que são textos curtos e obrigatórios), a `descricao` costuma ser um **texto longo e opcional**. Vamos analisar como tratar campos opcionais na validação e como integrá-los de forma completa à **busca textual multicampo**.

**2. Alteração do modelo e dados**

O objeto de produto passa a ter a estrutura completa:

```json
{
  "id": 1,
  "nome": "Notebook Pro",
  "preco": 3500.0,
  "marca": "Dell",
  "estoque": 15,
  "descricao": "Notebook de alta performance com tela OLED e processador octa-core."
}
```

- No **Express**, o POST e o PUT aceitam `descricao` (armazenando string tratada ou `""`/`null`).
- No **FastAPI**, o `ProdutoInput` inclui `descricao`:

<details>
<summary><strong>Ver alteração no req.body e persistência no Express</strong></summary>

```js
const { nome, preco, marca, estoque, descricao } = req.body;

const erros = validarProduto({ nome, preco, marca, estoque, descricao });
if (Object.keys(erros).length > 0) {
  return res.status(400).json({ detail: erros });
}

const novoProduto = {
  id: novoId,
  nome: nome.trim(),
  preco,
  marca: marca.trim(),
  estoque: Number(estoque),
  descricao: typeof descricao === "string" ? descricao.trim() : descricao ?? "",
};
```

Em `PUT`, o campo `descricao` é tratado como opcional: se vier como texto, é salvo já normalizado; se vier vazio, nulo ou ausente, o objeto continua válido.

</details>

<details>
<summary><strong>Ver ProdutoInput completo (FastAPI)</strong></summary>

```python
class ProdutoInput(BaseModel):
    nome: str | None = None
    preco: float | None = None
    marca: str | None = None
    estoque: int | None = None
    descricao: str | None = None
```

</details>

**3. Validação**

**Regras para o campo `descricao`:**
- **Opcional:** o cliente pode omitir o campo ou enviar `null`/`""`;
- **Se informada:** deve ser uma string;
- **Limite de tamanho:** no máximo **500 caracteres** (para evitar sobrecarga de dados no payload).

> ✏️ **Desafio:** Valide `descricao` como campo opcional: se fornecido, deve ser uma string de até 500 caracteres; se omitido ou nulo, deve ser aceito normalmente.

<details>
<summary><strong>Ver validação de descrição no Express</strong></summary>

Na função `validarProduto`:

```js
// Validação de descricao (opcional, máximo 500 caracteres se informada)
if (descricao !== undefined && descricao !== null) {
  if (typeof descricao !== "string") {
    erros.descricao = "O campo deve ser uma string.";
  } else if (descricao.trim().length > 500) {
    erros.descricao = "A descrição não pode ultrapassar 500 caracteres.";
  }
}
```

</details>

<details>
<summary><strong>Ver validação de descrição no FastAPI</strong></summary>

Na função `validar_produto`:

```python
# Validação de descricao (opcional, máximo 500 caracteres se informada)
if descricao is not None:
    if not isinstance(descricao, str):
        erros["descricao"] = "O campo deve ser uma string."
    elif len(descricao.strip()) > 500:
        erros["descricao"] = "A descrição não pode ultrapassar 500 caracteres."
```

</details>

**4. Filtro vs. Busca textual**

> **Por que não criamos um filtro exato `?descricao=...`?**
> Um filtro exato (`?marca=Dell`) faz sentido para atributos categóricos. Textos longos como descrições raramente são consultados por igualdade exata. O mecanismo adequado e natural para consultar descrições é a **busca textual (`search`)**. Criar um filtro exato de descrição seria redundante e pouco útil.

**5. Ordenação por descrição**

Permitir `ordering=descricao` e `ordering=-descricao`.

> ✏️ **Desafio:** Permita ordenar por `descricao` alfabeticamente, tratando produtos que não possuem descrição (ou com valor nulo) sem causar erros de execução.

<details>
<summary><strong>Ver ordenação por descrição no Express</strong></summary>

```js
} else if (campoOrdenacao === "descricao") {
  const descA = (a.descricao || "").toLowerCase();
  const descB = (b.descricao || "").toLowerCase();
  comparacao = descA.localeCompare(descB);
}
```

</details>

<details>
<summary><strong>Ver ordenação por descrição no FastAPI</strong></summary>

```python
elif campo_ordenacao == "descricao":
    resultado.sort(key=lambda p: (p.get("descricao") or "").lower(), reverse=ordem_desc)
```

</details>

**6. Busca textual multicampo (nome, marca, descricao)**

Agora nossa busca textual atinge seu formato mais poderoso. O parâmetro `?search=termo` pesquisa simultaneamente em todos os campos de texto:
1. **`nome`**
2. **`marca`**
3. **`descricao`**

Se o termo procurado for encontrado em **qualquer um** desses três campos, o produto é retornado.

> ✏️ **Desafio:** Atualize o filtro de busca (`search`) para verificar a presença do termo em `nome`, `marca` ou `descricao`.

<details>
<summary><strong>Ver busca multicampo no Express</strong></summary>

```js
if (search !== undefined && search !== "") {
  const termo = search.toLowerCase();
  resultado = resultado.filter(p => {
    const noNome = p.nome.toLowerCase().includes(termo);
    const naMarca = p.marca && p.marca.toLowerCase().includes(termo);
    const naDescricao = p.descricao && p.descricao.toLowerCase().includes(termo);
    return noNome || naMarca || naDescricao;
  });
}
```

</details>

<details>
<summary><strong>Ver busca multicampo no FastAPI</strong></summary>

```python
if search is not None:
    termo = search.lower()
    resultado = [
        p for p in resultado
        if termo in p["nome"].lower()
        or (p.get("marca") and termo in p["marca"].lower())
        or (p.get("descricao") and termo in p["descricao"].lower())
    ]
```

</details>

**Exemplos de busca:**

- `GET /api/produtos/?search=notebook` → localiza produtos cujo **nome** contém "notebook";
- `GET /api/produtos/?search=dell` → localiza produtos cuja **marca** é "Dell";
- `GET /api/produtos/?search=oled` → localiza produtos que possuem a palavra "oled" no meio da **descrição**, mesmo que a palavra não apareça no nome nem na marca!

**7. Contrato HTTP e testes no Bruno**

| Cenário de Teste | Método | URL | Corpo (JSON) | Status Esperado | O que validar |
| --- | --- | --- | --- | --- | --- |
| **01. Criar produto completo** | POST | `/api/produtos/` | `{"nome": "Monitor Pro 4K", "preco": 2800.0, "marca": "LG", "estoque": 8, "descricao": "Painel IPS com HDR e conexões USB-C"}` | `201 Created` | Retorna o produto com todos os 6 campos |
| **02. Criar produto sem descrição** | POST | `/api/produtos/` | `{"nome": "Cabo HDMI", "preco": 35.0, "marca": "Ugreen", "estoque": 50}` | `201 Created` | Sucesso (campo opcional) |
| **03. Descrição com mais de 500 chars** | POST | `/api/produtos/` | `{"nome": "X", "preco": 10.0, "marca": "Y", "estoque": 1, "descricao": "texto muito longo..."}` | `400 Bad Request` | `detail.descricao` acusa limite excedido |
| **04. Ordenar por descrição** | GET | `/api/produtos/?ordering=descricao` | — | `200 OK` | Ordena alfabeticamente pela descrição |
| **05. Buscar termo presente na descrição** | GET | `/api/produtos/?search=usb-c` | — | `200 OK` | Encontra o produto pela especificação na descrição |
| **06. Buscar termo presente na marca** | GET | `/api/produtos/?search=ugreen` | — | `200 OK` | Encontra pela marca |
| **07. Buscar termo presente no nome** | GET | `/api/produtos/?search=monitor` | — | `200 OK` | Encontra pelo nome |
| **08. Buscar termo em qualquer campo** | GET | `/api/produtos/?search=lg` | — | `200 OK` | Encontra por nome, marca ou descrição |

**8. O que observar**

- Campos opcionais exigem cuidado com valores ausentes (`null` ou `undefined`) para evitar exceções em tempo de execução ao ordenar ou buscar;
- A busca textual unificada oferece uma experiência excelente para o cliente da API sem a necessidade de múltiplos filtros complexos.

---

## 📘 Conclusão — O que mudou ao adicionar apenas três campos?

Ao final destas três aulas, a entidade `Produto` passou a contar com seis campos:

```text
id
nome
preco
marca
estoque
descricao
```

Observe a matriz de impacto de cada um dos novos campos adicionados:

| Campo | Validação | Filtro | Ordenação | Busca textual |
|---|---|---|---|---|
| **marca** | ✓ *(obrigatório, string, 2–50 chars)* | ✓ *(exato, case-insensitive)* | ✓ *(alfabética)* | ✓ *(participa do `search`)* |
| **estoque** | ✓ *(obrigatório, int, >= 0)* | ✓ *(intervalo: mín e máx)* | ✓ *(numérica)* | — *(não participa)* |
| **descricao** | ✓ *(opcional, string, máx 500 chars)* | — *(resolvido pela busca)* | ✓ *(alfabética)* | ✓ *(participa do `search`)* |

A principal conclusão prática desta etapa é:

> **Quanto mais informações uma entidade possui, mais comportamentos precisam ser considerados quando esses dados são expostos por uma API.**

Ao implementar esses três campos manualmente no Express e no FastAPI, percebemos que:
- Cada novo campo exige validações manuais repetitivas (`if`/`else`, checagem de tipo, limites);
- Para cada novo campo filtrável, precisamos extrair query params, tratar ausências e encadear filtros;
- A ordenação exige manter listas de campos permitidos e tratar tipos diferentes (número vs. string vs. nulos);
- A busca textual cresce em complexidade lógica a cada novo campo adicionado;
- Os testes precisam cobrir um número muito maior de combinações de requisições válidas e inválidas.

Esse crescimento de esforço manual prepara o terreno para a próxima etapa do nosso aprendizado: o **Django REST Framework (DRF)**. No DRF, veremos como abstrações poderosas (como **Models**, **Serializers**, **ModelViewSets** e **FilterBackends**) automatizam grande parte desse trabalho repetitivo de forma declarativa e padronizada.

---

# 🧭 Parte 7 — Django e Django REST Framework

Nas Partes 1–6 construímos uma API de produtos com Express e FastAPI. Agora vamos criar o projeto `django-bsi4` do zero e evoluí-lo em um único fluxo, da estrutura inicial até uma API REST completa.

O contrato continua familiar: `/api/produtos/`, com listagem, detalhe, criação, atualização e exclusão. A diferença está nas ferramentas integradas do Django: Model, ORM, migrations, SQLite, Admin e Django REST Framework. O cliente continua vendo os mesmos conceitos e URLs; o Django fornece abstrações mais declarativas para implementá-los.

## 📘 Aula 17 — Conhecendo o Django e criando o projeto

**Objetivo**

Criar o projeto `django-bsi4`, entender projeto e aplicação, executar o primeiro servidor e publicar o repositório que será usado nas aulas seguintes.

**Antes de começar**

Comece com uma pasta de trabalho vazia. Você já conhece endpoints e APIs pelas Partes 1–6; aqui o foco é a organização específica do Django.

**1. Instalando o uv**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**2. Criando o projeto**

```bash
mkdir django-bsi4
cd django-bsi4
uv init --app .
uv add django
uv run django-admin startproject config .
uv run python manage.py startapp produtos
```

O `startproject` cria a configuração global. O `startapp` cria uma aplicação, que reunirá o catálogo de produtos. Nesta aula instalamos somente o Django; DRF, documentação e filtros entram quando forem utilizados.

Em `config/settings.py`, acrescente `produtos` a `INSTALLED_APPS`, mantendo os aplicativos padrão que o Django já criou.

**3. Entendendo a estrutura**

- `manage.py`: ponto de entrada dos comandos do projeto;
- `config/settings.py`: configurações globais;
- `config/urls.py`: roteamento principal;
- `produtos/models.py`: modelos de dados;
- `produtos/views.py`: views e endpoints;
- `produtos/admin.py`: configuração do Admin;
- `produtos/migrations/`: histórico do esquema do banco.

Projeto é o conjunto de configurações. Aplicação é um módulo funcional dentro do projeto.

**4. Executando**

```bash
uv run python manage.py runserver
```

Acesse `http://127.0.0.1:8000/`. A página inicial confirma que o projeto funciona. Ainda não há Model de produto nem API REST.

**5. Versionando e publicando**

No VS Code, vá em **Source Control**, inicialize o repositório, crie-o no GitHub e publique.

**Resultado**

O projeto inicial está pronto para evoluir:

```text
django-bsi4/
├── manage.py
├── pyproject.toml
├── uv.lock
├── config/
└── produtos/
```

**💾 Commit sugerido**

Ao final da aula, depois de confirmar que o servidor abre, faça:

```bash
git add .
git commit -m "feat(django): cria projeto inicial"
```

## 📘 Aula 18 — Model, SQLite e migrations

**Objetivo**

Criar o Model inicial `Produto` com `nome` e `preco`, gerar uma migration e persistir a tabela no SQLite.

**1. Criando o Model**

Substitua `produtos/models.py` por:

```python
from django.db import models


class Produto(models.Model):
    nome = models.CharField(max_length=100)
    preco = models.DecimalField(max_digits=8, decimal_places=2)

    def __str__(self):
        return f"{self.nome} - R$ {self.preco}"
```

O campo `id` é criado automaticamente. `DecimalField` representa valores monetários com precisão decimal; o Model descreve a tabela, mas ainda não alterou o banco.

**2. Gerando e aplicando a migration**

```bash
uv run python manage.py makemigrations
uv run python manage.py migrate
uv run python manage.py showmigrations
uv run python manage.py check
```

`makemigrations` registra a mudança em `produtos/migrations/`. `migrate` aplica a mudança em `db.sqlite3`. `showmigrations` mostra o que foi aplicado e `check` verifica a consistência do projeto.

**3. Carregando dados**

Baixe o fixture inicial do repositório do tutorial para o diretório atual. No GitHub, o arquivo será publicado no branch `main`:

```bash
curl -L https://raw.githubusercontent.com/marrcandre/desweb2/main/produtos/produtos.json -o produtos.json
```

Esse arquivo contém os dados iniciais usados pelo tutorial. Os outros fixtures da pasta `produtos/` serão utilizados posteriormente, nas aulas em que o Model for evoluído.

```bash
uv run python manage.py loaddata produtos.json
```

Os produtos serão inseridos no banco. O ORM poderá consultar `Produto.objects.all()` sem SQL escrito manualmente.

**Resultado**

O projeto tem Model, migration, banco SQLite e dados persistidos.

**💾 Commit sugerido**

Depois de aplicar a migration e confirmar `showmigrations`:

```bash
git add .
git commit -m "feat(produtos): adiciona model e migrations"
```

## 📘 Aula 19 — Django Admin

**Objetivo**

Gerenciar produtos no painel administrativo antes de criar qualquer endpoint REST.

**1. Registrando o Model**

Edite `produtos/admin.py`:

```python
from django.contrib import admin

from .models import Produto


@admin.register(Produto)
class ProdutoAdmin(admin.ModelAdmin):
    list_display = ("id", "nome", "preco")
    search_fields = ("nome",)
```

`ModelAdmin` define como o Model aparece na interface. `list_display` escolhe colunas e `search_fields` habilita a pesquisa pelo nome.

**2. Criando e usando o acesso**

```bash
uv run python manage.py createsuperuser
uv run python manage.py runserver
```

Acesse `http://127.0.0.1:8000/admin/`, entre com o superusuário, cadastre produtos, edite um registro, pesquise pelo nome e exclua um registro de teste.

O Admin grava na mesma tabela SQLite usada pelo ORM e pela futura API. Ele não substitui a API: são interfaces diferentes sobre o mesmo Model e banco.

**💾 Commit sugerido**

Depois de registrar o Model e verificar o CRUD no Admin:

```bash
git add .
git commit -m "feat(admin): configura gerenciamento de produtos"
```

## 📘 Aula 20 — Primeiro endpoint com Django REST Framework

**Objetivo**

Criar o primeiro endpoint REST completo, já com CRUD, usando `ModelSerializer`, `ModelViewSet` e `DefaultRouter`.

**1. Instalando e configurando o DRF**

Instale o pacote somente agora:

```bash
uv add djangorestframework
```

O aplicativo `produtos` já foi adicionado na Aula 17. Agora, acrescente apenas `"rest_framework"` ao `INSTALLED_APPS`, preservando os aplicativos e as demais configurações que o Django já criou. Se o bloco `REST_FRAMEWORK` ainda não existir, crie-o; se já existir, acrescente as configurações indicadas ao bloco existente.

```python
"rest_framework",
```

**2. Criando o ModelSerializer**

Crie `produtos/serializers.py`:

```python
from rest_framework import serializers

from .models import Produto


class ProdutoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Produto
        fields = ("id", "nome", "preco")
```

O `ModelSerializer` representa instâncias como dados que podem virar JSON e valida entradas antes de salvar. Ele também aproveita os tipos e limites definidos no Model.

**3. Criando a ModelViewSet**

Substitua `produtos/views.py` por:

```python
from rest_framework.viewsets import ModelViewSet

from .models import Produto
from .serializers import ProdutoSerializer


class ProdutoViewSet(ModelViewSet):
    queryset = Produto.objects.all()
    serializer_class = ProdutoSerializer
```

A `ModelViewSet` disponibiliza automaticamente as ações de listagem, detalhe, criação, atualização completa (`PUT`), atualização parcial (`PATCH`) e exclusão. `PATCH` representa atualização parcial. Nesta etapa didática, os testes do contrato utilizam `PUT` para atualização completa; o `PATCH` existe porque faz parte das ações padrão da `ModelViewSet`, mas não será explorado neste momento.

**4. Registrando o DefaultRouter**

Substitua `config/urls.py` por:

```python
from django.contrib import admin
from django.urls import include, path
from rest_framework.routers import DefaultRouter

from produtos.views import ProdutoViewSet

router = DefaultRouter()
router.register("produtos", ProdutoViewSet, basename="produto")

urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/", include(router.urls)),
]
```

O Router gera as URLs a partir do registro da ViewSet. O padrão é:

```text
Model
  ↓
ModelSerializer
  ↓
ModelViewSet
  ↓
DefaultRouter
  ↓
Endpoint REST
```

A responsabilidade de cada camada é diferente: o Model cuida da persistência e do ORM; o Serializer cuida da representação e da validação; a ViewSet reúne as ações da API; e o Router gera as URLs.

**5. Testando o CRUD**

```text
GET    /api/produtos/
GET    /api/produtos/{id}/
POST   /api/produtos/
PUT    /api/produtos/{id}/
PATCH  /api/produtos/{id}/
DELETE /api/produtos/{id}/
```

Use o navegador para o GET e a interface navegável do DRF para testar criação, atualização parcial, atualização completa e exclusão. Ao final, a API funciona, mas ainda não possui documentação OpenAPI configurada.

**💾 Commit sugerido**

Depois de testar as seis operações do CRUD:

```bash
git add .
git commit -m "feat(drf): cria primeiro endpoint de produtos"
```

## 📘 Aula 21 — OpenAPI e Swagger

**Objetivo**

Adicionar documentação automática à API existente sem alterar sua arquitetura.

**1. Instalando e configurando o drf-spectacular**

```bash
uv add drf-spectacular
```

Em `config/settings.py`, acrescente o aplicativo `drf_spectacular` a `INSTALLED_APPS` e acrescente as configurações do schema. Preserve os aplicativos padrão, `rest_framework` e as demais configurações já existentes; não substitua todo o arquivo `settings.py`. Se `REST_FRAMEWORK` já existir, acrescente `DEFAULT_SCHEMA_CLASS` ao bloco sem apagar outras chaves:

```python
"drf_spectacular",

REST_FRAMEWORK = {
  # acrescente esta chave ao bloco existente, se ele já existir
    "DEFAULT_SCHEMA_CLASS": "drf_spectacular.openapi.AutoSchema",
}

SPECTACULAR_SETTINGS = {
    "TITLE": "API de Produtos",
    "DESCRIPTION": "API de produtos construída com Django REST Framework",
    "VERSION": "1.0.0",
}
```

**2. Criando as URLs da documentação**

Atualize `config/urls.py`, preservando o Router da Aula 20:

```python
from drf_spectacular.views import (
    SpectacularAPIView,
    SpectacularRedocView,
    SpectacularSwaggerView,
)

urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/", include(router.urls)),
    path("api/schema/", SpectacularAPIView.as_view(), name="schema"),
    path("api/docs/", SpectacularSwaggerView.as_view(url_name="schema"), name="swagger-ui"),
    path("api/redoc/", SpectacularRedocView.as_view(url_name="schema"), name="redoc"),
]
```

Abra `/api/schema/` para o documento OpenAPI, `/api/docs/` para o Swagger UI e `/api/redoc/` para o Redoc. O Swagger deve documentar o CRUD completo criado na Aula 20.

Aula 20: a API funciona. Aula 21: a API funciona e também tem documentação automática. Nenhuma ViewSet é criada ou substituída nesta aula.

**💾 Commit sugerido**

Depois de abrir `/api/docs/` e confirmar todas as operações:

```bash
git add .
git commit -m "docs(api): adiciona documentação OpenAPI e Swagger"
```

## 📘 Aula 22 — Validações

**Objetivo**

Adicionar regras de negócio ao `ProdutoSerializer` e confirmar que a validação acontece antes da persistência.

Atualize `produtos/serializers.py`:

```python
from decimal import Decimal

from rest_framework import serializers

from .models import Produto


class ProdutoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Produto
        fields = ("id", "nome", "preco")

    def validate_nome(self, value):
        nome_limpo = value.strip()
        if len(nome_limpo) < 2:
            raise serializers.ValidationError(
                "O nome deve possuir pelo menos 2 caracteres."
            )
        return nome_limpo

    def validate_preco(self, value):
        if value <= Decimal("0"):
            raise serializers.ValidationError("O preço deve ser maior que zero.")
        return value
```

`DecimalField` entrega um `Decimal`, por isso a comparação usa `Decimal("0")`, sem converter o valor monetário para `float`.

No Swagger, envie `nome: "A"` e `preco: 0`: a resposta deve ser `400 Bad Request`. Depois envie `{"nome": "Cabo USB", "preco": 20.00}` e confirme `201 Created`. O objeto só é salvo depois que todas as validações passam.

**💾 Commit sugerido**

Depois de testar casos válidos e inválidos no Swagger:

```bash
git add .
git commit -m "feat(api): adiciona validações de produtos"
```

## 📘 Aula 23 — Filtros

**Objetivo**

Filtrar produtos por faixa de preço usando `django-filter`.

**1. Instalando e configurando**

```bash
uv add django-filter
```

Acrescente `django_filters` a `INSTALLED_APPS`. No `REST_FRAMEWORK`, preserve `DEFAULT_SCHEMA_CLASS` e acrescente:

```python
"DEFAULT_FILTER_BACKENDS": [
    "django_filters.rest_framework.DjangoFilterBackend",
],
```

Crie `produtos/filters.py`:

```python
from django_filters import rest_framework as filters

from .models import Produto


class ProdutoFilter(filters.FilterSet):
    preco_minimo = filters.NumberFilter(field_name="preco", lookup_expr="gte")
    preco_maximo = filters.NumberFilter(field_name="preco", lookup_expr="lte")

    class Meta:
        model = Produto
        fields = ("preco_minimo", "preco_maximo")
```

Atualize `produtos/views.py`:

```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework.viewsets import ModelViewSet

from .filters import ProdutoFilter
from .models import Produto
from .serializers import ProdutoSerializer


class ProdutoViewSet(ModelViewSet):
    queryset = Produto.objects.all()
    serializer_class = ProdutoSerializer
    filter_backends = (DjangoFilterBackend,)
    filterset_class = ProdutoFilter
```

`gte` significa maior ou igual e `lte` significa menor ou igual. Teste:

```text
GET /api/produtos/?preco_minimo=100&preco_maximo=1000
GET /api/produtos/?preco_minimo=100
GET /api/produtos/?preco_maximo=1000
```

Confira no Swagger que os parâmetros aparecem no endpoint e teste também uma combinação sem resultados.

**💾 Commit sugerido**

Depois de testar os dois limites e o intervalo:

```bash
git add .
git commit -m "feat(api): adiciona filtros por preço"
```

## 📘 Aula 24 — Ordenação e busca textual

**Objetivo**

Combinar filtros, ordenação e busca textual no mesmo endpoint, mantendo somente os campos existentes nesta etapa.

Atualize `produtos/views.py`:

```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework.filters import OrderingFilter, SearchFilter
from rest_framework.viewsets import ModelViewSet

from .filters import ProdutoFilter
from .models import Produto
from .serializers import ProdutoSerializer


class ProdutoViewSet(ModelViewSet):
    queryset = Produto.objects.all()
    serializer_class = ProdutoSerializer
    filter_backends = (DjangoFilterBackend, SearchFilter, OrderingFilter)
    filterset_class = ProdutoFilter
    ordering_fields = ("nome", "preco")
    ordering = ("id",)
    search_fields = ("nome",)
```

`OrderingFilter` aceita `ordering=nome`, `ordering=-nome`, `ordering=preco` e `ordering=-preco`. `SearchFilter` faz busca parcial e sem diferenciar maiúsculas em `nome`.

```text
GET /api/produtos/?ordering=nome
GET /api/produtos/?ordering=-preco
GET /api/produtos/?search=mouse
GET /api/produtos/?preco_minimo=100&search=teclado&ordering=nome
```

Os três mecanismos se combinam no queryset: filtros estruturados, busca, ordenação e, na aula seguinte, paginação. `marca` e `descricao` entrarão na busca na Aula 26; `estoque` ficará fora por ser numérico.

**💾 Commit sugerido**

Depois de conferir consultas isoladas e combinadas no Swagger:

```bash
git add .
git commit -m "feat(api): adiciona busca e ordenação"
```

## 📘 Aula 25 — Paginação

**Objetivo**

Preservar no Django o contrato de paginação usado nas Partes Express/FastAPI:

```json
{
  "page": 1,
  "page_size": 10,
  "total_pages": 5,
  "results": []
}
```

O `PageNumberPagination` padrão do DRF usa `count`, `next`, `previous` e `results`. A classe abaixo continua usando `PageNumberPagination`, mas reformata a resposta e retorna uma lista vazia quando a página solicitada está além do limite.

Crie `produtos/pagination.py`:

Normalmente, não é necessário sobrescrever `paginate_queryset()` para usar paginação no DRF. Esta sobrescrita existe especificamente neste tutorial para preservar o contrato equivalente ao construído anteriormente com Express e FastAPI: quando o cliente solicita uma página além da última, a API deve retornar `200` com `results: []`, em vez do erro de página inválida que o comportamento padrão pode produzir.

```python
from math import ceil

from rest_framework.exceptions import ValidationError
from rest_framework.pagination import PageNumberPagination
from rest_framework.response import Response


class ProdutoPagination(PageNumberPagination):
    page_size_query_param = "page_size"
    max_page_size = 100

    def paginate_queryset(self, queryset, request, view=None):
        self.request = request
    page_size_param = request.query_params.get(self.page_size_query_param)
    if page_size_param is not None:
      try:
        if int(page_size_param) > self.max_page_size:
          raise ValidationError({
            "page_size": "O campo page_size não pode passar de 100."
          })
      except ValueError:
        pass

        page_size = self.get_page_size(request)
        if page_size is None:
            return None

        self.paginator = self.django_paginator_class(queryset, page_size)
        try:
            self.page_number = int(request.query_params.get(self.page_query_param, 1))
        except (TypeError, ValueError):
            raise ValidationError({"page": "Informe um número inteiro positivo."})

        if self.page_number < 1:
            raise ValidationError({"page": "Informe um número inteiro positivo."})

        self.total_pages = ceil(self.paginator.count / page_size)
        if self.total_pages == 0 or self.page_number > self.total_pages:
            self.page = None
            return []

        self.page = self.paginator.page(self.page_number)
        return list(self.page)

    def get_paginated_response(self, data):
        return Response({
            "page": self.page_number,
            "page_size": self.paginator.per_page,
            "total_pages": self.total_pages,
            "results": data,
        })
```

A classe respeita `page_size`, rejeita valores acima de `100` com `400` e `detail`, valida páginas menores que 1 e calcula `total_pages` antes do corte. Assim, uma página além do limite produz `200` com `results: []`, como nas Partes 1–6.

**Configurando globalmente**

Em `config/settings.py`, acrescente estas chaves ao bloco `REST_FRAMEWORK` existente, preservando as demais configurações. Se o bloco ainda não existir, crie-o:

```python
"DEFAULT_PAGINATION_CLASS": "produtos.pagination.ProdutoPagination",
"PAGE_SIZE": 10,
```

**Testando**

```text
GET /api/produtos/?page=1
GET /api/produtos/?page=2&page_size=20
GET /api/produtos/?search=mouse&ordering=-preco&page=1&page_size=5
GET /api/produtos/?page=999
```

A última requisição deve retornar `200` com `results` vazia. Teste também `page_size=101`, que deve retornar `400` com `detail`; valores até 100 seguem normalmente. Filtros, busca e ordenação são aplicados ao queryset antes da paginação, exatamente como no contrato anterior.

**💾 Commit sugerido**

Depois de verificar o formato da resposta, o limite de tamanho e as combinações:

```bash
git add .
git commit -m "feat(api): adiciona paginação customizada"
```

## 📘 Aula 26 — Exercício: Evoluindo o Produto

**Objetivo**

Repetir no Django a evolução feita nas Aulas 14–16 e perceber que uma mudança no Model atravessa todas as camadas:

```text
Model
→ migration
→ serializer
→ validação
→ filtro
→ ordenação
→ busca
→ Swagger
→ teste
```

Os exercícios são cumulativos. Depois de cada campo, gere e aplique a migration, atualize o código, confira o Swagger e execute os testes.

**1. Marca**

`marca` será obrigatório, terá de 2 a 50 caracteres e participará do filtro exato, da ordenação e da busca.

**Desafio:** adicione o campo ao Model, ao Serializer e ao `ProdutoFilter`, e atualize `ordering_fields` e `search_fields`.

<details>
<summary>Ver solução</summary>

No Model, use um valor temporário para os registros existentes:

```python
marca = models.CharField(max_length=50, default="Genérica")
```

```bash
uv run python manage.py makemigrations
uv run python manage.py migrate
```

No serializer:

```python
marca = serializers.CharField(required=True, max_length=50)

# Meta.fields: ("id", "nome", "preco", "marca")
def validate_marca(self, value):
    marca_limpa = value.strip()
    if len(marca_limpa) < 2:
        raise serializers.ValidationError(
            "A marca deve possuir entre 2 e 50 caracteres."
        )
    return marca_limpa
```

No FilterSet:

```python
marca = filters.CharFilter(field_name="marca", lookup_expr="iexact")
```

Inclua `marca` em `Meta.fields`, em `ordering_fields` e em `search_fields`. O filtro exato aceita, por exemplo, `?marca=dell`, sem diferenciar maiúsculas.
</details>

Teste `POST` válido, `POST` com marca curta, `GET /api/produtos/?marca=dell`, `?ordering=marca` e `?search=dell`.

**2. Estoque**

`estoque` será inteiro, obrigatório, não negativo, filtrável por intervalo e ordenável. Ele **não** participará da busca textual.

**Desafio:** adicione o campo, gere a migration e atualize serializer, validação, filtro e ordenação.

<details>
<summary>Ver solução</summary>

```python
estoque = models.IntegerField(default=0)
```

```bash
uv run python manage.py makemigrations
uv run python manage.py migrate
```

No serializer, inclua `estoque` em `Meta.fields` e declare-o obrigatório:

```python
estoque = serializers.IntegerField(required=True)

def validate_estoque(self, value):
    if value < 0:
        raise serializers.ValidationError("O estoque não pode ser negativo.")
    return value
```

No FilterSet:

```python
estoque_minimo = filters.NumberFilter(field_name="estoque", lookup_expr="gte")
estoque_maximo = filters.NumberFilter(field_name="estoque", lookup_expr="lte")
```

Inclua os dois filtros em `Meta.fields` e `estoque` em `ordering_fields`. Não inclua `estoque` em `search_fields`; quantidades são consultadas por `estoque_minimo` e `estoque_maximo`.
</details>

Teste `estoque=0` (válido), `-1`, `"dez"`, `5.5`, `?estoque_minimo=10&estoque_maximo=30` e `?ordering=-estoque`. O campo inteiro do Model e o campo gerado pelo ModelSerializer rejeitam tipos incompatíveis antes da persistência.

**3. Descrição**

`descricao` será opcional, aceitará `null` ou texto vazio, terá no máximo 500 caracteres, será ordenável e participará da busca.

**Desafio:** adicione o campo e atualize serializer, validação, ordenação e busca.

<details>
<summary>Ver solução</summary>

```python
descricao = models.TextField(blank=True, null=True)
```

```bash
uv run python manage.py makemigrations
uv run python manage.py migrate
```

No serializer, inclua `descricao` em `Meta.fields` e use:

```python
def validate_descricao(self, value):
    if value is not None and len(value.strip()) > 500:
        raise serializers.ValidationError(
            "A descrição não pode ultrapassar 500 caracteres."
        )
    return value
```

Finalize as listas da ViewSet:

```python
ordering_fields = ("nome", "preco", "marca", "estoque", "descricao")
search_fields = ("nome", "marca", "descricao")
```

Não é necessário filtro exato para descrição: textos longos são consultados pela busca. `NULL` é tratado pelo banco durante ordenação e não é acessado como string no Python.
</details>

Teste `POST` sem descrição, texto acima de 500 caracteres, `?search=usb-c` e `?ordering=descricao`. Depois de cada campo, confirme no Swagger que o schema, os parâmetros e as operações do CRUD continuam atualizados.

**💾 Commit sugerido**

Depois de concluir os três campos e testar o ciclo completo:

```bash
git add .
git commit -m "feat(produtos): adiciona marca estoque e descricao"
```

A Parte 7 termina com o mesmo conceito das Partes 1–6, mas com outra implementação: o Express e o FastAPI resolvem várias etapas manualmente; o Django combina ORM, migrations, serializers, ViewSets, routers e filter backends para declarar o mesmo contrato `/api/produtos/`.
