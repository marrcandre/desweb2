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

**6. Atividade Prática**

**Objetivo:** Usar   ou Thunder Client para explorar APIs públicas úteis.

1. Escolher **uma** API da lista acima.
2. Fazer uma requisição **GET**.
3. Registrar:
- URL utilizada
- Método HTTP
- Status code
- Conteúdo JSON retornado

---

**7. Atividades de Fixação**

Escolha **uma API pública diferente das apresentadas** e:
- Faça **3 requisições** (GET, POST e DELETE, se suportado).
- Salve prints das requisições e respostas.
- Anote status code e conteúdo retornado.

---

**8. Sugestões:**
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
        {"id": 1, "nome": "Mouse", "preco": 89.90},
        {"id": 2, "nome": "Teclado", "preco": 199.90}
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

---

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
items = [Item(id=1, nome="Teclado", preco=199.90), Item(id=2, nome="Mouse", preco=89.90)]

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

**5. Discussão**

* O conceito de rota, método e resposta é **igual** em ambas as stacks.
* Diferenças estão apenas na sintaxe e ferramentas.
* Importância de entender HTTP antes de aprender frameworks.

---

**Atividade de Fixação**

* Adicionar uma rota **DELETE** em ambas as implementações, removendo um produto pelo id.
* Testar no **Postman** ou **Thunder Client** e registrar o status code.

---

