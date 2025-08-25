**DESENVOLVIMENTO WEB II**

**OBJETIVO GERAL**

Capacitar o aluno a projetar, desenvolver, documentar e publicar APIs REST, utilizando Django + Django REST Framework como framework de referência, mas compreendendo conceitos aplicáveis a qualquer stack de desenvolvimento.

**OBJETIVOS ESPECÍFICOS**
1. Entender os fundamentos de APIs, HTTP e REST.
2. Implementar CRUDs com Django REST Framework utilizando `ModelViewSet` e `ModelSerializer`.
3. Trabalhar com autenticação, permissões, filtros, paginação e upload de arquivos.
4. Documentar APIs utilizando ferramentas compatíveis com o padrão OpenAPI (Swagger).
5. Publicar projetos em serviços de deploy modernos (Render, Supabase, AWS, etc.).
6. Trabalhar com versionamento de código e boas práticas de commits no GitHub.
7. Desenvolver projeto final com requisitos mínimos, mas liberdade para escolha de tecnologias.

**CONTEÚDO PROGRAMÁTICO**

1. Fundamentos de APIs
- O que é uma API
- HTTP: métodos, status codes, headers
- JSON e serialização
- Diferenças entre APIs REST, GraphQL e outras arquiteturas
2. Introdução ao Django REST Framework
- Criação de projeto e app
- Configuração inicial e boas práticas
3. Modelos e Serializers
- `ModelSerializer` e campos customizados
4. Views e ViewSets
- `ModelViewSet` e actions personalizadas
5. URLs e Roteamento
- `routers` e `paths` customizados
6. Autenticação e Permissões
- SessionAuth, TokenAuth, JWT
- Permissões customizadas
7. Filtros, Ordenação e Paginação
8. Upload de Arquivos e Integração com Cloudinary
9. Documentação de APIs
- Swagger/OpenAPI (`drf-spectacular` ou `drf-yasg`)
10. Boas Práticas e Padrões
- Estrutura de projeto
- Versionamento no Git
11. Deploy
- Render, Supabase, AWS, Railway
12. Projeto Final
- Requisitos mínimos:
  - CRUD
  - Autenticação e permissões
  - Upload de arquivos
  - Filtros e paginação
  - Documentação
  - Deploy
- Liberdade de stack (Node, Flask, Laravel, etc.)
13. Apresentação e Avaliação dos Projetos

---

**LINKS ÚTEIS**

- Tutorial de Django: https://github.com/marrcandre/django-drf-tutorial
- Documentação do Django: https://docs.djangoproject.com/
- Documentação oficial do Django REST Framework: https://www.django-rest-framework.org/
- Roadmap de backend: https://roadmap.sh/backend
- Vídeos:
  - [A Forma Ideal de Projetos Web | Os 12 Fatores - Fábio Akita](https://www.youtube.com/watch?v=gpJgtED36U4)
  - [Série "Começando aos 40 - Fábio Akita"](https://www.youtube.com/watch?v=O76ZfAIEukE&list=PLjuQ-0yGqLjcFmMkiYvHPraSptrhPlOuK)


---

# Aula 1 – Introdução à Disciplina e Fundamentos de APIs

**Objetivo da Aula**

Compreender o que é uma API, como funciona o protocolo HTTP e como interagir com APIs públicas que trazem informações úteis para o dia a dia.

---

**Conteúdo**

**1. Apresentação da disciplina**

- Ementa, objetivos e formas de avaliação.
- Projeto final e avaliação teórica no meio do semestre.
- Expectativas e forma de trabalho (teoria + prática).

---

**2. O que é uma API**

**[API](https://aws.amazon.com/what-is/api/)** (*Application Programming Interface*) é um conjunto de regras e definições que permite que sistemas diferentes se comuniquem.
Ela define **como** um sistema pode ser usado por outro.

- Exemplos do dia a dia:
  - Aplicativo de clima pegando dados de um serviço meteorológico.
  - Aplicativo de transporte usando mapas do Google.
  - Sites de e-commerce consultando meios de pagamento.
- **API Web**: acessada via internet, geralmente usando o protocolo HTTP.
- **Cliente** (quem faz a requisição) ↔ **Servidor** (quem responde com os dados).

---

**3. REST – Princípios básicos**

**[REST](https://aws.amazon.com/what-is/api/#seo-faq-pairs#what-are-rest-apis)** (*Representational State Transfer*) é um conjunto de boas práticas para criar APIs web.
Principais características:
1. **Stateless** – cada requisição contém todas as informações necessárias; o servidor não guarda o “estado” do cliente.
2. **Recursos identificados por URL** – cada tipo de dado tem um endereço único (ex: `/usuarios/1` para o usuário com id 1).
3. **Operações com métodos HTTP** – ações claras para cada tipo de operação.
4. **Formato de dados padronizado** – normalmente **JSON** (mas pode incluir outros formatos, como XML).

**Principais métodos HTTP e uso comum**:
- **GET** → Ler dados.
  Ex: `GET /usuarios`
- **POST** → Criar novos dados.
  Ex: `POST /usuarios`
- **PUT** → Atualizar completamente um recurso existente.
  Ex: `PUT /usuarios/1`
- **PATCH** → Atualizar parcialmente um recurso existente.
  Ex: `PATCH /usuarios/1`
- **DELETE** → Remover dados.
  Ex: `DELETE /usuarios/1`

---

**4. Status codes HTTP**

O servidor sempre responde com um **código numérico** indicando o resultado da requisição.

**Principais categorias**:
- **1xx – Informativo** (raramente usado diretamente pelo dev)
- **2xx – Sucesso**
  - `200 OK` → Requisição bem-sucedida
  - `201 Created` → Recurso criado com sucesso
  - `204 No Content` → Sucesso sem conteúdo no corpo da resposta
- **3xx – Redirecionamento**
  - `301 Moved Permanently`
  - `302 Found`
- **4xx – Erros do cliente**
  - `400 Bad Request` → Requisição inválida
  - `401 Unauthorized` → Autenticação necessária
  - `403 Forbidden` → Acesso negado
  - `404 Not Found` → Recurso não encontrado
  - `409 Conflict` → Conflito de requisição
- **5xx – Erros do servidor**
  - `500 Internal Server Error`
  - `503 Service Unavailable`

---

**5. Demonstração – APIs Públicas Úteis**

Vamos explorar APIs reais que trazem dados práticos para Araquari/SC.

1. **Consultar um CEP (ViaCEP):** https://viacep.com.br/ws/89211120/json/

2. **Cotação do dólar (AwesomeAPI):**
https://economia.awesomeapi.com.br/json/last/USD-BRL

3. **Previsão do tempo (Open-Meteo):**
- Latitude/Longitude de Araquari: **-26.3747**, **-48.7181**
https://api.open-meteo.com/v1/forecast?latitude=-26.3747&longitude=-48.7181&current_weather=true


Cada requisição será feita usando **navegador**, **Postman** e **Thunder Client**, para comparar resultados.

---

**Atividade Prática**

**Objetivo:** Usar   ou Thunder Client para explorar APIs públicas úteis.

1. Escolher **uma** API da lista acima.
2. Fazer uma requisição **GET**.
3. Registrar:
- URL utilizada
- Método HTTP
- Status code
- Conteúdo JSON retornado

---

**Atividades de Fixação**

Escolha **uma API pública diferente das apresentadas** e:
- Faça **3 requisições** (GET, POST e DELETE, se suportado).
- Salve prints das requisições e respostas.
- Anote status code e conteúdo retornado.

---

**Sugestões:**
- [BrasilAPI](https://brasilapi.com.br/) (CNPJs, feriados, dados de bancos)
- [IBGE – Localidades](https://servicodados.ibge.gov.br/api/docs/localidades)
- [Cat Facts](https://catfact.ninja/) (fatos aleatórios sobre gatos)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) (API falsa para testes)

---

**Recursos e Links**
- [Roadmap.sh - Backend](https://roadmap.sh/backend)
- [O que é uma API](https://aws.amazon.com/what-is/api/)?
- [Postman](https://www.postman.com/)

---

# Aula 2 – Estrutura e Teste de Requisições HTTP

**Objetivo**

Compreender como estruturar e testar uma API simples em duas tecnologias diferentes (Python + FastAPI e Node.js + Express), reforçando que os conceitos de HTTP e REST são independentes da stack utilizada.

---

**Conteúdo**

**1. Revisão rápida**

- Métodos HTTP (GET, POST, PUT/PATCH, DELETE).
- Status codes.
- Estrutura de uma resposta JSON.

---

**2. Criando uma API com FastAPI (Python)**

**Objetivo:** Montar um servidor que responda a requisições de teste.

**Instalação**

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install fastapi uvicorn
```

**Código - `main.py`**

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Modelo para POST
class Item(BaseModel):
    nome: str
    preco: float

# Rota GET
@app.get("/produtos")
def listar_produtos():
    return [
    {"id": 1, "nome": "Notebook", "preco": 3500},
    {"id": 2, "nome": "Mouse", "preco": 80},
    {"id": 3, "nome": "Teclado", "preco": 150},
    {"id": 4, "nome": "Monitor", "preco": 1200},
    {"id": 5, "nome": "Impressora", "preco": 300},
    ]

# Rota POST
@app.post("/produtos")
def criar_produto(item: Item):
    return {"mensagem": "Produto criado com sucesso", "dados": item}

# Rodar servidor:
# uvicorn main:app --reload
```

**Rodar servidor:**

```bash
uvicorn main:app --reload
```

**Explicação do código**

- **FastAPI:** Framework para construção de APIs em Python.
- **Pydantic:** Biblioteca para validação de dados e criação de modelos.
- **Item:** Modelo que representa um produto, com campos para nome e preço.
- **Rotas:**
  - `GET /produtos`: Retorna uma lista de produtos.
  - `POST /produtos`: Cria um novo produto a partir dos dados enviados no corpo da requisição.
- **Execução:** Para rodar o servidor, utilize o comando `uvicorn main:app --reload`.
- **Testes:** Utilize ferramentas como **Postman** ou **Thunder Client** para testar as rotas da API.
- **Documentação:** Acesse a documentação automática gerada pelo FastAPI em `http://localhost:8000/docs`.
- **Exemplos de Requisições:**
  - `GET /produtos`: Retorna todos os produtos.
  - `POST /produtos`: Cria um novo produto.

**Um exemplo mais completo**

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Modelo para itens
class Item(BaseModel):
    id: int
    nome: str
    preco: float

# Modelo para POST
class ItemInput(BaseModel):
    nome: str
    preco: float

# Modelo para resposta de POST
class ItemInputResponse(BaseModel):
    message: str
    dados: Item

# Lista de itens em memória
items = [
    Item(id=1, nome="Notebook", preco=3500.00),
    Item(id=2, nome="Mouse", preco=80.00),
    Item(id=3, nome="Teclado", preco=150.00),
    Item(id=4, nome="Monitor", preco=1200.00),
    Item(id=5, nome="Impressora", preco=300.00),
]

# Rota GET
@app.get("/produtos", response_model=list[Item])
def listar_produtos():
    return [
       {"id": item.id, "nome": item.nome, "preco": item.preco} for item in items
    ]

# Rota POST
@app.post("/produtos", response_model=ItemInputResponse)
def criar_produto(item: ItemInput):
    novo_id = max(item.id for item in items) + 1
    novo_item = Item(id=novo_id, **item.model_dump())
    items.append(novo_item)
    response = ItemInputResponse(message="Produto criado com sucesso", dados=novo_item)
    return response.model_dump()

# Rodar servidor:
# uvicorn main:app --reload
```

**Diferenças neste código**

- **Modelos:** Foram adicionados modelos mais específicos para entrada e saída de dados, utilizando o Pydantic.
- **Rota GET:** A rota GET agora utiliza o modelo de resposta para garantir que a estrutura dos dados retornados esteja correta.
- **Rota POST:** A rota POST agora utiliza um modelo de entrada e um modelo de resposta, melhorando a validação e a documentação automática da API.
- **Lista em memória:** A lista de itens é mantida em memória, permitindo a adição de novos produtos via POST.
- **Validação:** O Pydantic garante que os dados enviados nas requisições estejam no formato correto, retornando erros de validação automaticamente quando necessário.
- **Documentação:** A documentação automática gerada pelo FastAPI é atualizada com base nos modelos utilizados, facilitando a compreensão da API.

---

**3. Criando uma API com Node.js + Express**

**Objetivo:** Implementar o mesmo comportamento usando JavaScript.

**Instalação**

```bash
npm init -y
npm install express
```

**Código - `index.js`**

```javascript
const express = require('express');
const app = express();

app.use(express.json());

// Rota GET
app.get('/produtos', (req, res) => {
  res.json([
    { id: 1, nome: 'Mouse', preco: 89.90 },
    { id: 2, nome: 'Teclado', preco: 199.90 }
  ]);
});

// Rota POST
app.post('/produtos', (req, res) => {
  res.json({
    mensagem: 'Produto criado com sucesso',
    dados: req.body
  });
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

// Rodar servidor:
// node index.js
```

**Rodar servidor:**

```bash
node index.js
```

**Um exemplo mais completo**

```javascript
const express = require('express');
const app = express();

app.use(express.json());

// "Banco de dados" em memória
let produtos = [
  { id: 1, nome: 'Teclado', preco: 199.90 },
  { id: 2, nome: 'Mouse', preco: 89.90 }
];

// Rota GET
app.get('/produtos', (req, res) => {
  res.json([...produtos]); // cópia para evitar alteração acidental
});

// Rota POST
app.post('/produtos', (req, res) => {
  const novoId = produtos.length > 0 ? Math.max(...produtos.map(p => p.id)) + 1 : 1;
  const novoProduto = { id: novoId, ...req.body };

  produtos.push(novoProduto);

  res.status(201).json({
    message: 'Produto criado com sucesso',
    dados: novoProduto
  });
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));
```

**Explicação do código**

Temos agora o código Express com a mesma lógica do seu exemplo em FastAPI, inclusive mantendo:

* Lista de produtos em memória.
* POST que adiciona e retorna o novo item junto com mensagem.
* Geração automática de id.

**Diferenças em relação ao FastAPI:**

* No Express, não temos tipagem automática nem validação de entrada por padrão (equivalente ao que o Pydantic faz no FastAPI).
* A rota POST usa req.body diretamente, por isso seria interessante depois incluir uma validação para manter a mesma segurança de tipos.
* A estrutura da resposta (message + dados) foi mantida idêntica à versão Python para facilitar comparação lado a lado.

---

**4. Testando com Postman / Thunder Client**

* Fazer requisição GET para /produtos.
* Fazer requisição POST para /produtos com corpo JSON:

```json
{
  "nome": "Monitor",
  "preco": 1299.90
}
```

* Comparar:
  * Estrutura da resposta.
  * Status code retornado.
  * Diferenças na implementação entre FastAPI e Express.

---

**Discussão**

* O conceito de rota, método e resposta é **igual** em ambas as stacks.
* Diferenças estão apenas na sintaxe e ferramentas.
* Importância de entender HTTP antes de aprender frameworks.

---

**Atividade de Fixação**

* Adicionar uma rota **DELETE** em ambas as implementações, removendo um produto pelo id.
* Testar no **Postman** ou **Thunder Client** e registrar o status code.

---

# Aula 3 – Rotas com parâmetros e retorno estruturado

**Objetivo**

Ampliar o conhecimento sobre APIs explorando rotas dinâmicas com parâmetros e retornos mais elaborados em JSON, preparando o terreno para integração com banco de dados.

---


**Revisão da aula anterior**

Relembrar rapidamente:
- Diferença entre GET e POST.
- Corpo de requisição (body) e retorno (response).
- Conceito de status code e mensagens de resposta.

---
**Conteúdo**

**1. Introdução aos parâmetros de rota e query**

**Conceitos:**

- **Parâmetros de rota (path params):** parte da URL que representa um dado variável.
  - Ex.: `/produtos/123` → `123` é o ID do produto.

- **Parâmetros de query (query params):** enviados na URL após `?` para filtros, ordenação e paginação.
  - Ex.: `/produtos?categoria=eletronicos&ordem=preco`.

**Boas práticas:**

- Usar parâmetros de rota para identificar recursos.
- Usar parâmetros de query para filtragem, paginação e ordenação.

---

**2. Demonstração prática – FastAPI**

**Exemplo de aplicação:**

```python
from fastapi import FastAPI
from typing import Optional

app = FastAPI()

produtos = [
    {"id": 1, "nome": "Notebook", "preco": 3500},
    {"id": 2, "nome": "Mouse", "preco": 80},
    {"id": 3, "nome": "Teclado", "preco": 150},
    {"id": 4, "nome": "Monitor", "preco": 1200},
    {"id": 5, "nome": "Impressora", "preco": 300},
]

@app.get("/produtos/{id_produto}")
def get_produto(id_produto: int):
    for produto in produtos:
        if produto["id"] == id_produto:
            return produto
    return {"erro": "Produto não encontrado"}

@app.get("/produtos")
def listar_produtos(categoria: Optional[str] = None):
    # Como ainda não temos banco, o filtro é apenas ilustrativo
    return produtos
```

**Testar no navegador e no Postman:**

- GET http://localhost:8000/produtos/1
- GET http://localhost:8000/produtos?categoria=eletronico

---

**3. Demonstração prática – Node.js com Express**

```javascript
const produtos = [
  { id: 1, nome: 'Notebook', preco: 3500 },
  { id: 2, nome: 'Mouse', preco: 120 },
  { id: 3, nome: 'Teclado', preco: 250 }
];

app.get('/produtos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const produto = produtos.find(p => p.id === id);
  if (produto) {
    res.json(produto);
  } else {
    res.status(404).json({ erro: 'Produto não encontrado' });
  }
});

app.get('/produtos', (req, res) => {
  // Filtro fictício
  res.json(produtos);
});

app.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});
```

**Testar no navegador e no Postman:**

- GET http://localhost:3000/produtos/1
- GET http://localhost:3000/produtos?categoria=eletronicos

---

**Exercício prático**

- Criar uma rota que busque produtos com base em **query params** `min_preco` e `max_preco`.
- Implementar no **FastAPI** ou **Express**.
- Testar com **Postman**.

# Aula 4 – CRUD Completo em APIs REST

**Objetivo da Aula:**

Implementar operações completas de CRUD em APIs REST, utilizando FastAPI e Node.js + Express, reforçando conceitos de HTTP, rotas, status codes e resposta estruturada.

---

**Revisão rápida**

- Conceitos revisados:
  - Métodos HTTP: GET, POST, PUT, DELETE
  - Status codes: 200, 201, 204, 404
  - Estrutura de resposta JSON

- Pergunta:
  - _"Como podemos manipular dados de uma lista de produtos usando apenas requisições HTTP?"_

---

**Conteúdo**

**1. CRUD completo com FastAPI**

**Código exemplo:**

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

# Modelo para itens
class Item(BaseModel):
    id: int
    nome: str
    preco: float

# Modelo para entrada de dados (POST/PUT)
class ItemInput(BaseModel):
    nome: str
    preco: float

# Modelo para resposta de POST
class ItemInputResponse(BaseModel):
    message: str
    dados: Item

# Lista de itens em memória
items = [
    Item(id=1, nome="Notebook", preco=3500.00),
    Item(id=2, nome="Mouse", preco=80.00),
    Item(id=3, nome="Teclado", preco=150.00),
    Item(id=4, nome="Monitor", preco=1200.00),
    Item(id=5, nome="Impressora", preco=300.00),
]

# Listar todos os produtos (GET)
@app.get("/produtos", response_model=list[Item])
def listar_produtos():
    return items

# Listar produto por ID (GET)
@app.get("/produtos/{item_id}", response_model=Item)
def listar_produto_por_id(item_id: int):
    for item in items:
        if item.id == item_id:
            return item
    raise HTTPException(status_code=404, detail="Produto não encontrado")

# Criar novo produto (POST)
@app.post("/produtos", response_model=ItemInputResponse)
def criar_produto(item: ItemInput):
    novo_id = max(item_existente.id for item_existente in items) + 1
    novo_item = Item(id=novo_id, **item.model_dump())
    items.append(novo_item)
    return ItemInputResponse(message="Produto criado com sucesso", dados=novo_item)

# Atualizar produto (PUT)
@app.put("/produtos/{item_id}", response_model=ItemInputResponse)
def atualizar_produto(item_id: int, item_atualizado: ItemInput):
    for i, item in enumerate(items):
        if item.id == item_id:
            items[i] = Item(id=item_id, **item_atualizado.model_dump())
            return ItemInputResponse(message="Produto atualizado com sucesso", dados=items[i])
    raise HTTPException(status_code=404, detail="Produto não encontrado")

# Remover produto (DELETE)
@app.delete("/produtos/{item_id}")
def remover_produto(item_id: int):
    for i, item in enumerate(items):
        if item.id == item_id:
            items.pop(i)
            return {"message": "Produto removido com sucesso"}
    raise HTTPException(status_code=404, detail="Produto não encontrado")

# Rodar servidor:
# uvicorn aula4_crud_completo:app --reload

# Acesse a documentação da API em http://localhost:8000/docs
```

**Demonstração prática:**

- Rodar o servidor com: `uvicorn main:app --reload`
- Testar todas as rotas usando **Postman** ou **Thunder Client**:
  - `GET /produtos` → listar todos
  - `GET /produtos/1` → listar por id
  - `POST /produtos` → criar novo produto
  - `PUT /produtos/1` → atualizar produto
  - `DELETE /produtos/1` → remover produto

---

**2. CRUD completo com Express**

**Código exemplo:**

```javascript
const express = require('express');
const app = express();
app.use(express.json());

let produtos = [
    { id: 1, nome: "Notebook", preco: 3500.00 },
    { id: 2, nome: "Mouse", preco: 80.00 },
    { id: 3, nome: "Teclado", preco: 150.00 },
    { id: 4, nome: "Monitor", preco: 1200.00 },
    { id: 5, nome: "Impressora", preco: 300.00 }
];

app.get('/produtos', (req, res) => res.json(produtos));

app.get('/produtos/:id', (req, res) => {
    const produto = produtos.find(p => p.id === parseInt(req.params.id));
    if (!produto) return res.status(404).json({ error: "Produto não encontrado" });
    res.json(produto);
});

app.post('/produtos', (req, res) => {
    const { nome, preco } = req.body;
    const novoId = produtos.length ? Math.max(...produtos.map(p => p.id)) + 1 : 1;
    const novoProduto = { id: novoId, nome, preco };
    produtos.push(novoProduto);
    res.json({ message: "Produto criado com sucesso", dados: novoProduto });
});

app.put('/produtos/:id', (req, res) => {
    const index = produtos.findIndex(p => p.id === parseInt(req.params.id));
    if (index === -1) return res.status(404).json({ error: "Produto não encontrado" });
    produtos[index] = { id: parseInt(req.params.id), ...req.body };
    res.json({ message: "Produto atualizado com sucesso", dados: produtos[index] });
});

app.delete('/produtos/:id', (req, res) => {
    const index = produtos.findIndex(p => p.id === parseInt(req.params.id));
    if (index === -1) return res.status(404).json({ error: "Produto não encontrado" });
    produtos.splice(index, 1);
    res.json({ message: "Produto removido com sucesso" });
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));
```

---

**Demonstração prática:**

- Rodar o servidor: `node index.js`
- Testar todas as rotas no Postman ou **Thunder Client**, da mesma forma que no FastAPI.

---

**Discussão**

- Comparar FastAPI e Express:
  - Sintaxe diferente, mesma lógica de CRUD.
  - Status codes, rotas e payloads JSON são iguais.
  - FastAPI gera documentação automática; Express depende de ferramentas externas (Swagger, Postman Collection).
- Reforçar conceitos de **HTTP**, **REST** e **boas práticas**.

---

**Atividade prática**

- Adicionar no projeto:
  - Filtrar produtos por min_preco e max_preco (query params).
  - Implementar validação básica de dados (preço > 0, nome não vazio).
- Testar todas as rotas e anotar status codes e respostas.

---

# Aula 5 – CRUD com validação, filtros, paginação e ordenação (FastAPI)

**Objetivo da Aula:**

Implementar um CRUD completo com validação de dados, filtros, paginação e ordenação em FastAPI, reforçando conceitos de boas práticas em APIs REST.

---

**Revisão rápida:**

- CRUD básico da Aula anterio.
- Relembrar: métodos HTTP, status codes e resposta JSON.
- Pergunta:
  - _"Se nossa lista tiver 10 mil produtos, como facilitar a busca e listagem?"_

---
**Conteúdo**

**1. CRUD com validação**

- Validação no **Pydantic**: `nome` não pode ser vazio, `preco` deve ser maior que zero.
- Exemplo:

```python
from pydantic import BaseModel, Field

class ItemInput(BaseModel):
    nome: str = Field(..., min_length=1, description="Nome não pode ser vazio")
    preco: float = Field(..., gt=0, description="Preço deve ser maior que zero")
```

**2. Listagem com filtros**

- `min_preco` e `max_preco` como parâmetros de query.
- Exemplo de rota:

```python
@app.get("/produtos", response_model=list[Item])
def listar_produtos(min_preco: float | None = None, max_preco: float | None = None):
    resultado = items
    if min_preco is not None:
        resultado = [item for item in resultado if item.preco >= min_preco]
    if max_preco is not None:
        resultado = [item for item in resultado if item.preco <= max_preco]
    return resultado
```

**3. Paginação**

- Adicionar parâmetros `pagina` e `por_pagina`.
- Exemplo:

```python
@app.get("/produtos", response_model=list[Item])
def listar_produtos(pagina: int = 0, por_pagina: int = 10):
    return items[pagina * por_pagina : (pagina + 1) * por_pagina]
```

**4. Ordenação**

- Parâmetro `ordenar_por` (nome | preco) e `ordem` (asc | desc).
- Exemplo:

```python
@app.get("/produtos", response_model=list[Item])
def listar_produtos(
    ordenar_por: str = "id",
    ordem: str = "asc",
):
    resultado = items
    resultado.sort(key=lambda x: getattr(x, ordenar_por), reverse=(ordem == "desc"))
```

**5. Combinação dos recursos**

- Filtros, paginação e ordenação juntos na mesma rota.
- Discussão:
  - _"O código está ficando repetitivo e verboso. Como frameworks mais completos (como DRF) ajudam a reduzir isso?"_

---

**Atividade prática**

- Exercício:
- Implementar validação, filtros, paginação e ordenação juntos.
- Testar diferentes combinações no Swagger UI (http://localhost:8000/docs).

**Conclusão**

À medida em que vamos o sistema vai crescendo e vamos implementando mais recursos, é importante manter a organização e a clareza do código. O uso de frameworks e boas práticas de desenvolvimento pode ajudar a gerenciar a complexidade e a escalabilidade da aplicação.

---

# Aula 6 – CRUD com validação, filtros, paginação e ordenação (Express)

**Objetivo da Aula:**

Implementar um CRUD completo com validação de dados, filtros, paginação e ordenação em Express, reforçando a comparação entre Python (FastAPI) e JavaScript (Node.js/Express).

---
**Revisão rápida:**

- CRUD básico com Express (**Aula 4**).
- Na **Aula 5** fizemos isso no FastAPI com validações, filtros e paginação.
- Pergunta para reflexão:
  - *"Qual código ficou mais verboso: FastAPI ou Express?"*

---

**Conteúdo**

**1. CRUD com validação**

- No Express fazemos validações manualmente.
- Exemplo de validação no `POST`:

```javascript
if (!nome || nome.trim() === "") {
    return res.status(400).json({ error: "O nome não pode ser vazio" });
}
if (typeof preco !== "number" || preco <= 0) {
    return res.status(400).json({ error: "O preço deve ser maior que zero" });
}
```

**2. Listagem com filtros**

- Podemos passar parâmetros de query:
  - `min_preco` → filtra produtos com preço mínimo.
  - `max_preco` → filtra produtos com preço máximo.

Exemplo:

```javascript
if (min_preco) resultado = resultado.filter(p => p.preco >= parseFloat(min_preco));
if (max_preco) resultado = resultado.filter(p => p.preco <= parseFloat(max_preco));
```

**3. Paginação**

- Usamos `pagina` e `por_pagina`.
- Implementação:

```javascript
pagina = parseInt(pagina) || 0;
por_pagina = parseInt(por_pagina) || resultado.length;
resultado = resultado.slice(pagina, pagina + por_pagina);
```

**4. Ordenação**

- `ordenar_por` → nome ou preço.
- `ordem` → asc ou desc.

Exemplo:

```javascript
if (ordenar_por) {
    resultado.sort((a, b) => {
        if (a[ordenar_por] < b[ordenar_por]) return ordem === "desc" ? 1 : -1;
        if (a[ordenar_por] > b[ordenar_por]) return ordem === "desc" ? -1 : 1;
        return 0;
    });
}
```

---

**Atividade prática**

- Testar a API com os seguintes endpoints:

  - **Listar todos os produtos:** `http://localhost:8000/produtos`
  - **Ordenar por preço (crescente):** `http://localhost:8000/produtos?ordenar_por=preco&ordem=asc`
  - **Ordenar por nome (decrescente):** `http://localhost:8000/produtos?ordenar_por=nome&ordem=desc`
  - **Filtrar por preço mínimo:** `http://localhost:8000/produtos?min_preco=100`
  - **Filtrar por preço máximo:** `http://localhost:8000/produtos?max_preco=1000`
  - **Filtrar por preço mínimo e máximo:** `http://localhost:8000/produtos?min_preco=100&max_preco=1000`
  - **Paginação:** `http://localhost:8000/produtos?pagina=1&por_pagina=2`

---

**Conclusão**

- No **FastAPI (Python)**, usamos **Pydantic** para validação automática.
- No **Express (Node.js)**, precisamos implementar manualmente as validações.
- O código em **Express** tende a ser **mais verboso e repetitivo**, mas também é **mais flexível**.

# Aula 7 