**Desenvolvimento WEB II**

# Disciplina: Desenvolvimento Web II

##  Objetivo Geral
Capacitar o aluno a projetar, desenvolver, documentar e publicar APIs REST, utilizando Django + Django REST Framework como framework de referência, mas compreendendo conceitos aplicáveis a qualquer stack de desenvolvimento.

##  Objetivos Específicos
1. Entender os fundamentos de APIs, HTTP e REST.
2. Implementar CRUDs com Django REST Framework utilizando `ModelViewSet` e `ModelSerializer`.
3. Trabalhar com autenticação, permissões, filtros, paginação e upload de arquivos.
4. Documentar APIs utilizando ferramentas compatíveis com o padrão OpenAPI (Swagger).
5. Publicar projetos em serviços de deploy modernos (Render, Supabase, AWS, etc.).
6. Trabalhar com versionamento de código e boas práticas de commits no GitHub.
7. Desenvolver projeto final com requisitos mínimos, mas liberdade para escolha de tecnologias.

## Conteúdo Programático

### 1. Fundamentos de APIs
- O que é uma API
- HTTP: métodos, status codes, headers
- JSON e serialização
- Diferenças entre APIs REST, GraphQL e outras arquiteturas

### 2. Introdução ao Django REST Framework
- Criação de projeto e app
- Configuração inicial e boas práticas

### 3. Modelos e Serializers
- `ModelSerializer` e campos customizados

### 4. Views e ViewSets
- `ModelViewSet` e actions personalizadas

### 5. URLs e Roteamento
- `routers` e `paths` customizados

### 6. Autenticação e Permissões
- SessionAuth, TokenAuth, JWT
- Permissões customizadas

### 7. Filtros, Ordenação e Paginação

### 8. Upload de Arquivos e Integração com Cloudinary

### 9. Documentação de APIs
- Swagger/OpenAPI (`drf-spectacular` ou `drf-yasg`)

### 10. Boas Práticas e Padrões
- Estrutura de projeto
- Versionamento no Git

### 11. Deploy
- Render, Supabase, AWS, Railway

### 12. Projeto Final
- Requisitos mínimos:
  - CRUD
  - Autenticação e permissões
  - Upload de arquivos
  - Filtros e paginação
  - Documentação
  - Deploy
- Liberdade de stack (Node, Flask, Laravel, etc.)

### 13. Apresentação e Avaliação dos Projetos

---

# Aula 1 – Introdução à Disciplina e Fundamentos de APIs

## Objetivo da Aula
Compreender o que é uma API, como funciona o protocolo HTTP e como interagir com APIs públicas que trazem informações úteis para o dia a dia.

---

## Conteúdo

### 1. Apresentação da disciplina
- Ementa, objetivos e formas de avaliação.
- Projeto final e avaliação teórica no meio do semestre.
- Expectativas e forma de trabalho (teoria + prática).

---

### 2. O que é uma API
**API** (*Application Programming Interface*) é um conjunto de regras e definições que permite que sistemas diferentes se comuniquem.
Ela define **como** um sistema pode ser usado por outro.

- Exemplos do dia a dia:
  - Aplicativo de clima pegando dados de um serviço meteorológico.
  - Aplicativo de transporte usando mapas do Google.
  - Sites de e-commerce consultando meios de pagamento.
- **API Web**: acessada via internet, geralmente usando o protocolo HTTP.
- **Cliente** (quem faz a requisição) ↔ **Servidor** (quem responde com os dados).

---

### 3. REST – Princípios básicos
**REST** (*Representational State Transfer*) é um conjunto de boas práticas para criar APIs web.
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

### 4. Status codes HTTP
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

### 5. Demonstração – APIs Públicas Úteis
Vamos explorar APIs reais que trazem dados práticos para Araquari/SC.

1. **Consultar um CEP (ViaCEP):** https://viacep.com.br/ws/89211120/json/

2. **Cotação do dólar (AwesomeAPI):**
https://economia.awesomeapi.com.br/json/last/USD-BRL

3. **Previsão do tempo (Open-Meteo):**
- Latitude/Longitude de Araquari: **-26.3747**, **-48.7181**
https://api.open-meteo.com/v1/forecast?latitude=-26.3747&longitude=-48.7181&current_weather=true


Cada requisição será feita usando **navegador**, **Postman** e **Insomnia**, para comparar resultados.

---

## Atividade Prática
**Objetivo:** Usar Postman ou Insomnia para explorar APIs públicas úteis.

1. Escolher **uma** API da lista acima.
2. Fazer uma requisição **GET**.
3. Registrar:
- URL utilizada
- Método HTTP
- Status code
- Conteúdo JSON retornado

---

## Atividades de Fixação
Escolha **uma API pública diferente das apresentadas** e:
- Faça **3 requisições** (GET, POST e DELETE, se suportado).
- Salve prints das requisições e respostas.
- Anote status code e conteúdo retornado.

Sugestões:
- [BrasilAPI](https://brasilapi.com.br/) (CNPJs, feriados, dados de bancos)
- [IBGE – Localidades](https://servicodados.ibge.gov.br/api/docs/localidades)
- [Cat Facts](https://catfact.ninja/) (fatos aleatórios sobre gatos)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) (API falsa para testes)

---

## Recursos e Links
- [Postman](https://www.postman.com/)
- [Insomnia](https://insomnia.rest/)
