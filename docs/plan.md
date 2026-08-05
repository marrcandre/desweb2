# Planejamento das APIs e sequência didática — Desenvolvimento Web 2

## 1. Objetivo deste documento

Este documento registra as decisões pedagógicas e técnicas para o desenvolvimento da etapa de APIs da disciplina **Desenvolvimento Web 2**.

As três tecnologias principais serão:

* **Express**
* **FastAPI**
* **Django REST Framework (DRF)**

O objetivo não é ensinar três APIs diferentes, mas utilizar as três tecnologias para demonstrar **diferentes formas de construir a mesma API**.

A ideia central é:

> **compreender os fundamentos → construir uma API manualmente → utilizar frameworks com diferentes níveis de abstração → comparar as soluções → consumir a API → aplicar os conhecimentos em um projeto próprio.**

A etapa inicial deve ser suficientemente completa para fornecer uma base sólida, mas deliberadamente enxuta para preservar tempo para o projeto individual dos alunos.

---

# 2. Princípio pedagógico central

A disciplina deve priorizar conceitos que possam ser transferidos para diferentes tecnologias.

O aluno deve compreender:

* HTTP;
* requisição e resposta;
* métodos HTTP;
* URLs;
* parâmetros de rota;
* parâmetros de consulta;
* JSON;
* códigos de status;
* REST;
* CRUD;
* validação;
* filtros;
* busca;
* ordenação;
* paginação;
* persistência;
* documentação de APIs;
* consumo de APIs.

O objetivo não é apresentar uma quantidade excessiva de recursos de um framework.

A ideia é que o aluno consiga compreender:

> **o que uma API faz e por que ela funciona dessa maneira, antes de aprender como determinado framework implementa isso.**

---

# 3. Estrutura geral do conteúdo

A disciplina será organizada por **etapas conceituais**, e não simplesmente por uma sequência de arquivos ou aulas.

A metodologia adotada para Express e FastAPI será **progressiva e alternada**.

Isso significa que um determinado conceito será primeiro implementado no Express e, imediatamente depois, implementado no FastAPI.

Somente então será apresentado o próximo conceito.

A visão geral será:

```text
FUNDAMENTOS
    ↓
HTTP / REST / JSON / CRUD
    ↓
API-BASE
    ↓
Express: conceito
    ↓
FastAPI: mesmo conceito
    ↓
Comparação
    ↓
Próximo conceito
    ↓
Express: implementação
    ↓
FastAPI: implementação
    ↓
Comparação
    ↓
...
    ↓
Django REST Framework
    ↓
Evolução da API
    ↓
Consumo da API
    ↓
Vue.js
    ↓
Projeto individual
```

Essa organização evita que o Express se torne uma etapa excessivamente longa e também evita que o FastAPI seja percebido pelos alunos apenas como uma repetição de tudo aquilo que já foi feito.

A comparação entre as tecnologias acontece **durante o processo de construção**, e não somente ao final.

---

# 4. API comum às três tecnologias

Express, FastAPI e Django REST Framework deverão representar essencialmente a **mesma API de Produtos** durante a etapa inicial.

Isso permitirá comparar:

* a mesma entidade;
* as mesmas rotas;
* os mesmos métodos HTTP;
* os mesmos parâmetros;
* os mesmos filtros;
* a mesma busca;
* a mesma ordenação;
* a mesma paginação;
* respostas equivalentes;
* códigos HTTP equivalentes.

A principal diferença deverá ser:

> **como cada tecnologia resolve o mesmo problema.**

Isso é mais importante pedagogicamente do que fazer cada tecnologia possuir uma API diferente.

O contrato comum será construído progressivamente. Não é necessário implementar todos os recursos desde o primeiro arquivo.

O Django poderá posteriormente evoluir além desse contrato inicial para demonstrar recursos próprios de modelagem e abstração.

---

# 5. Modelo inicial

A primeira versão da API trabalhará deliberadamente com um modelo simples:

```text
Produto
├── id
├── nome
└── preco
```

Neste momento, **não serão adicionados** ao modelo comum:

* descrição;
* estoque;
* categoria;
* relacionamento entre entidades;
* datas de criação;
* datas de atualização.

Esses recursos poderão aparecer posteriormente como uma evolução, especialmente no Django REST Framework.

A simplicidade inicial é intencional.

O objetivo é garantir que o aluno compreenda o funcionamento da API antes de lidar com uma quantidade maior de entidades e relacionamentos.

---

# 6. Contrato da API

As três APIs deverão utilizar o mesmo padrão de URLs.

## Base

```text
/api/produtos/
```

## Listar produtos

```http
GET /api/produtos/
```

## Buscar produto por ID

```http
GET /api/produtos/{id}/
```

## Criar produto

```http
POST /api/produtos/
```

## Atualizar produto

```http
PUT /api/produtos/{id}/
```

## Excluir produto

```http
DELETE /api/produtos/{id}/
```

A adoção da barra final aproxima Express e FastAPI do padrão utilizado pelo Django REST Framework.

O objetivo é que o aluno perceba que o contrato HTTP pode permanecer o mesmo, mesmo quando a implementação muda completamente.

Esse contrato será introduzido progressivamente ao longo das etapas.

### PUT

O `PUT` será tratado como **atualização completa do recurso**.

Isso significa que a requisição deverá fornecer todos os campos necessários do recurso:

```json
{
    "nome": "Notebook",
    "preco": 3500.00
}
```

O `PATCH` **não fará parte da etapa inicial**.

Ele poderá ser abordado posteriormente caso seja pedagogicamente necessário.

---

# 7. Evolução da listagem

A listagem será introduzida de forma progressiva.

Cada conceito será implementado primeiro no Express e, em seguida, no FastAPI.

---

## 7.1 Listagem simples

Inicialmente:

```http
GET /api/produtos/
```

deve simplesmente retornar os produtos.

Exemplo:

```json
[
    {
        "id": 1,
        "nome": "Notebook",
        "preco": 3500.00
    },
    {
        "id": 2,
        "nome": "Mouse",
        "preco": 80.00
    }
]
```

Nesse momento, o foco será compreender:

* GET;
* endpoint;
* JSON;
* lista de objetos;
* resposta HTTP.

Primeiro esse comportamento será construído no Express.

Logo depois, será construído o equivalente no FastAPI.

A comparação ocorrerá nesse momento, enquanto o conceito ainda está recente.

---

## 7.2 Filtros

Depois da listagem básica, serão introduzidos filtros.

Inicialmente:

```text
preco_minimo
preco_maximo
```

Exemplo:

```http
GET /api/produtos/?preco_minimo=100&preco_maximo=1000
```

A sequência didática será:

```text
Express → filtros
        ↓
FastAPI → filtros
```

Os projetos deverão apresentar comportamento equivalente.

---

## 7.3 Busca

Depois dos filtros, será introduzida a busca textual.

A primeira versão deverá permitir pesquisar pelo nome do produto.

Exemplo conceitual:

```http
GET /api/produtos/?search=mouse
```

A implementação poderá ser diferente em cada tecnologia, mas o comportamento observado pelo cliente deverá ser equivalente.

A sequência será:

```text
Express → busca
        ↓
FastAPI → busca
```

---

## 7.4 Ordenação

Depois será introduzida a ordenação.

A nomenclatura será:

```text
ordering
```

Exemplos:

```http
GET /api/produtos/?ordering=nome
```

```http
GET /api/produtos/?ordering=-preco
```

O sinal `-` representa ordenação decrescente, aproximando o comportamento do padrão utilizado pelo Django REST Framework.

Inicialmente, os campos disponíveis para ordenação serão:

* `nome`;
* `preco`.

A sequência será:

```text
Express → ordenação
        ↓
FastAPI → ordenação
```

---

## 7.5 Paginação

A paginação será introduzida depois que filtros, busca e ordenação estiverem compreendidos.

Será adotado como padrão comum o modelo de paginação baseado em **número da página**, semelhante ao `PageNumberPagination` do Django REST Framework.

A decisão é utilizar **um único contrato de paginação para Express, FastAPI e Django**, permitindo que o aluno aprenda uma única forma de solicitar e interpretar páginas de resultados.

Os parâmetros serão:

```text
page
page_size
```

Exemplos:

```http
GET /api/produtos/?page=1
```

```http
GET /api/produtos/?page=2&page_size=20
```

O comportamento padrão será:

```text
page_size padrão: 10
page_size máximo: 100
```

Portanto:

* se `page_size` não for informado, serão retornados até 10 itens;
* o cliente poderá solicitar outro tamanho utilizando `page_size`;
* nenhum cliente poderá solicitar mais de 100 itens por página.

### Contrato da resposta paginada

Uma listagem paginada deverá retornar um objeto JSON com a seguinte estrutura:

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
        },
        {
            "id": 2,
            "nome": "Mouse",
            "preco": 80.00
        }
    ]
}
```

Os campos possuem os seguintes significados:

| Campo         | Significado                                  |
| ------------- | -------------------------------------------- |
| `page`        | número da página atual                       |
| `page_size`   | quantidade de itens solicitada para a página |
| `total_pages` | quantidade total de páginas disponíveis      |
| `results`     | coleção de produtos daquela página           |

Não será necessário incluir inicialmente:

```text
count
next
previous
```

A intenção é manter o contrato pequeno e suficiente para os objetivos pedagógicos da disciplina.

### Combinação com filtros, busca e ordenação

A paginação deverá funcionar em conjunto com os demais parâmetros de consulta.

Exemplo:

```http
GET /api/produtos/?search=mouse&ordering=-preco&page=2&page_size=10
```

A ordem conceitual do processamento será:

```text
dados
  ↓
filtros
  ↓
busca
  ↓
ordenação
  ↓
paginação
  ↓
resposta
```

Isso permitirá demonstrar que a paginação não substitui filtros, busca ou ordenação: ela é aplicada sobre o conjunto de resultados produzido por essas operações.

### Implementação nas tecnologias

No Express:

```text
paginação implementada manualmente
```

No FastAPI:

```text
paginação implementada explicitamente
```

No Django REST Framework:

```text
PageNumberPagination
        ↓
CustomPagination
```

A implementação poderá ser diferente, mas o contrato observado pelo cliente deverá ser equivalente.

A sequência será:

```text
Express → paginação
        ↓
FastAPI → paginação
        ↓
Comparação
        ↓
Django → paginação com abstração do DRF
```

O princípio é:

> **O aluno não deve precisar aprender três formas diferentes de solicitar a mesma página de resultados.**

---

# 8. CRUD

A API deverá implementar o CRUD completo:

```text
Create  → POST
Read    → GET
Update  → PUT
Delete  → DELETE
```

A progressão pedagógica será:

```text
GET lista
    ↓
GET por ID
    ↓
POST
    ↓
PUT
    ↓
DELETE
```

Cada conceito será implementado primeiro no Express e imediatamente depois no FastAPI.

Depois que o CRUD estiver compreendido, serão adicionados progressivamente:

```text
validação
    ↓
filtros
    ↓
busca
    ↓
ordenação
    ↓
paginação
```

A ordem dos conceitos é mais importante do que a numeração dos arquivos.

---

# 9. Validação

As três implementações deverão possuir regras equivalentes.

## Nome

O campo `nome`:

* é obrigatório;
* deve ser uma string;
* deve ser submetido a `trim`;
* não pode ficar vazio após o `trim`;
* deve possuir entre **2 e 100 caracteres** após o `trim`.

Exemplos válidos:

```text
Notebook
Mouse
Teclado mecânico
```

Exemplos inválidos:

```text
""
" "
"A"
```

---

## Preço

O campo `preco`:

* é obrigatório;
* deve ser numérico;
* deve ser **maior que zero**;
* deve possuir no máximo **2 casas decimais**.

Exemplos válidos:

```text
80
80.5
80.50
3500.00
```

Exemplos inválidos:

```text
0
-10
80.555
```

O objetivo é mostrar que **a mesma regra de negócio pode ser implementada de formas diferentes**.

### Express

Validação explícita no código.

### FastAPI

Validação utilizando Pydantic.

### Django REST Framework

Validação realizada pelo serializer/modelo.

A comparação entre essas três abordagens será parte importante da aprendizagem.

No caso de Express e FastAPI, a comparação será feita imediatamente após a implementação do mesmo requisito.

---

# 10. Formato das respostas

As respostas de sucesso deverão utilizar **JSON direto**, sem envelopes genéricos como `data`, `dados` ou `message`.

Para operações sobre um único recurso, como criação, consulta individual ou atualização, será retornado diretamente o objeto:

```json
{
    "id": 6,
    "nome": "Webcam",
    "preco": 250.00
}
```

Entretanto, a **listagem de produtos será paginada** e, portanto, utilizará o contrato específico definido na seção 7.5:

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
        },
        {
            "id": 2,
            "nome": "Mouse",
            "preco": 80.00
        }
    ]
}
```

Não será utilizado como padrão:

```json
{
    "message": "Produto criado com sucesso",
    "dados": {
        "id": 6,
        "nome": "Webcam",
        "preco": 250.00
    }
}
```

A intenção é aproximar as APIs de um padrão REST simples e facilitar o consumo posterior pelo Vue.js.

A diferença entre:

```text
GET /api/produtos/{id}/
```

e:

```text
GET /api/produtos/
```

é intencional:

```text
recurso individual
→ objeto JSON direto

coleção
→ objeto JSON contendo os metadados da paginação e results
```

---

# 11. Formato dos erros

As três APIs deverão utilizar um formato de erro comum.

Para erros gerais:

```json
{
    "detail": "Produto não encontrado."
}
```

Para erros de validação, `detail` poderá conter informações estruturadas por campo:

```json
{
    "detail": {
        "nome": "O nome deve possuir entre 2 e 100 caracteres.",
        "preco": "O preço deve ser maior que zero."
    }
}
```

A implementação interna poderá variar entre Express, FastAPI e Django REST Framework, mas o comportamento observado pelo cliente deverá ser equivalente.

O objetivo é evitar que cada tecnologia apresente um formato completamente diferente para representar o mesmo tipo de erro.

---

# 12. Códigos HTTP

As três APIs deverão procurar utilizar códigos HTTP equivalentes.

| Situação                     | Código |
| ---------------------------- | -----: |
| GET realizado com sucesso    |  `200` |
| POST realizado com sucesso   |  `201` |
| PUT realizado com sucesso    |  `200` |
| DELETE realizado com sucesso |  `204` |
| Dados inválidos              |  `400` |
| Produto não encontrado       |  `404` |

O objetivo é reforçar que os códigos de status fazem parte do protocolo HTTP e não são uma característica específica de Express, FastAPI ou Django.

---

# 13. Persistência

A persistência será introduzida progressivamente.

## Express

Inicialmente:

```text
dados em memória
```

Posteriormente:

```text
arquivo JSON
```

## FastAPI

Inicialmente:

```text
dados em memória
```

Posteriormente:

```text
arquivo JSON
```

## Django

Desde a etapa correspondente:

```text
SQLite
    ↓
Django ORM
```

Essa diferença é deliberada.

No Express e FastAPI, o aluno poderá perceber explicitamente o trabalho necessário para gerenciar os dados.

No Django, poderá observar como o ORM abstrai boa parte desse trabalho.

---

# 14. Express

O Express terá um papel importante na compreensão da construção de uma API de maneira relativamente manual.

Os alunos deverão trabalhar diretamente com conceitos como:

* `req`;
* `res`;
* rotas;
* métodos HTTP;
* parâmetros de rota;
* query string;
* body JSON;
* códigos HTTP;
* validação;
* manipulação dos dados;
* persistência;
* paginação.

O objetivo não é tornar o Express uma API excessivamente sofisticada.

Ele deve servir para tornar explícito aquilo que acontece quando uma requisição chega ao servidor.

O Express será, portanto, a primeira implementação de cada novo conceito.

---

# 15. FastAPI

O FastAPI será apresentado imediatamente depois do Express em cada etapa conceitual.

Seu papel será mostrar como uma abordagem mais declarativa e integrada pode reduzir parte do trabalho manual.

Deverá permitir trabalhar com:

* rotas;
* parâmetros;
* query parameters;
* path parameters;
* body;
* modelos Pydantic;
* validação;
* respostas;
* documentação automática;
* persistência simples;
* paginação.

A comparação com Express deverá mostrar principalmente como tipagem, validação e documentação podem ser integradas ao framework.

A intenção não é ensinar primeiro todo o Express e somente depois repetir todo o conteúdo no FastAPI.

A estratégia será:

```text
Express → conceito
FastAPI → mesmo conceito
Comparação
      ↓
próximo conceito
```

---

# 16. Estrutura dos arquivos didáticos

Nos exemplos de Express e FastAPI será mantida a estratégia de criar **arquivos progressivos**, permitindo que o aluno visualize a evolução do código.

Os arquivos representarão etapas do desenvolvimento.

A numeração deverá ser **correspondente entre Express e FastAPI**, sempre que as duas tecnologias estiverem tratando o mesmo conceito.

A convenção adotada será:

```text
NN-descricao
```

Por exemplo:

```text
01-api-basica
02-get-lista
03-get-id
04-post
05-put
06-delete
07-validacao
08-filtros
09-busca
10-ordenacao
11-paginacao
```

As extensões dependerão da tecnologia:

```text
Express
01-api-basica.js
02-get-lista.js
03-get-id.js
```

e:

```text
FastAPI
01-api-basica.py
02-get-lista.py
03-get-id.py
```

A numeração definitiva deverá respeitar os arquivos existentes e o mapa de correspondência estabelecido durante a revisão do material atual.

Não se deve criar uma sequência artificial apenas para obter números consecutivos.

O mais importante é que cada arquivo represente uma evolução pedagógica clara.

## Versão completa

Ao final da sequência de cada tecnologia deverá existir uma versão que represente a API completa construída naquela etapa.

A convenção será:

```text
NN-api-completa
```

Exemplos:

```text
12-api-completa.js
```

e:

```text
12-api-completa.py
```

Essa nomenclatura substitui as convenções anteriores `-final` e `-completo`.

A versão `NN-api-completa` representa o estado completo da API ao final da sequência trabalhada.

### Motivo da estratégia

Essa abordagem foi escolhida por ser simples para o contexto da disciplina.

O aluno consegue:

* abrir os arquivos;
* comparar versões;
* voltar para uma etapa anterior;
* observar exatamente o que mudou;
* entender a evolução do código.

Não será necessário depender exclusivamente de tags ou histórico do Git para acompanhar a evolução pedagógica.

Os arquivos de código continuarão sendo **evolutivos**, mesmo que a documentação utilize uma organização conceitual diferente.

---

# 17. Organização da documentação didática

A documentação da disciplina será organizada em **um único README**, com capítulos numerados sequencialmente.

Não serão criados vários READMEs fragmentados para cada aula.

A organização interna será feita utilizando seções e subseções.

Por exemplo:

```text
# Desenvolvimento Web 2 — APIs

## 1. Fundamentos HTTP

## 2. APIs REST

## 3. API de Produtos

## 4. Express
### 4.1 Rotas
### 4.2 CRUD
### 4.3 Validação
### 4.4 Filtros

## 5. FastAPI
### 5.1 Rotas
### 5.2 Modelos e validação
### 5.3 CRUD
### 5.4 Filtros

## 6. Comparação Express × FastAPI

## 7. Django REST Framework
### 7.1 Models
### 7.2 ORM
### 7.3 Serializers
### 7.4 ViewSets
### 7.5 Routers

## 8. Evolução da API

## 9. Consumo de APIs

## 10. Vue.js

## 11. Projeto individual
```

A numeração definitiva dos capítulos poderá ser ajustada conforme a organização final do conteúdo.

O princípio, entretanto, está definido:

> **um único README, capítulos numerados sequencialmente e seções internas para organizar os assuntos.**

A documentação não deverá depender da numeração dos arquivos de código.

Os arquivos podem continuar evoluindo independentemente da numeração dos capítulos do README.

Essa separação é intencional:

```text
README
→ organização conceitual e sequencial da disciplina

Arquivos de código
→ evolução prática de cada implementação
```

---

# 18. Django REST Framework

O Django REST Framework será apresentado como uma evolução daquilo que já foi construído manualmente.

A ideia não é simplesmente repetir a API do Express e do FastAPI.

O aluno deverá perceber como o framework permite estruturar e automatizar responsabilidades que anteriormente precisaram ser implementadas manualmente.

Os principais conceitos serão:

* Model;
* ORM;
* migrations;
* Serializer;
* ViewSet;
* Router;
* filtros;
* busca;
* ordenação;
* paginação;
* documentação OpenAPI;
* administração.

---

# 19. Modelo inicial do Django

A recomendação atual é que o Django também comece com a mesma entidade básica:

```text
Produto
- id
- nome
- preco
```

Isso permitirá uma comparação direta:

```text
Express
Produto(id, nome, preco)

FastAPI
Produto(id, nome, preco)

Django
Produto(id, nome, preco)
```

A diferença estará na forma de implementar.

O Django não deverá ser artificialmente reduzido apenas para permanecer igual às implementações anteriores.

A igualdade inicial existe para facilitar a comparação.

Depois disso, o Django poderá evoluir de acordo com suas próprias vantagens e possibilidades pedagógicas.

---

# 20. Evolução posterior do Django

A aplicação Django atualmente já possui recursos que vão além do modelo inicial.

Entre eles:

```text
descricao
estoque
criado_em
atualizado_em
categoria
```

e uma entidade:

```text
Categoria
```

com relacionamento `ForeignKey`.

Esses recursos **não precisam ser removidos**.

Eles poderão ser utilizados posteriormente para demonstrar uma evolução natural da API.

A progressão poderá ser:

```text
Produto básico
    ↓
novos campos
    ↓
Categoria
    ↓
relacionamento entre entidades
    ↓
filtros mais sofisticados
    ↓
API mais completa
```

A decisão importante é que **não devemos adicionar Categoria, estoque, relacionamentos ou outros recursos ao Express e ao FastAPI apenas para igualá-los ao Django**.

O Django poderá avançar além da API-base justamente para demonstrar suas capacidades de abstração e modelagem.

---

# 21. Comparação entre as tecnologias

Uma das partes mais importantes da disciplina será comparar as soluções.

A tabela inicial poderá seguir esta estrutura:

| Conceito     | Express      | FastAPI              | Django REST Framework  |
| ------------ | ------------ | -------------------- | ---------------------- |
| Rota         | `app.get()`  | `@app.get()`         | Router/ViewSet         |
| POST         | `app.post()` | `@app.post()`        | `create()`             |
| Validação    | Manual       | Pydantic             | Serializer/Model       |
| Persistência | Manual       | Manual               | ORM                    |
| Filtros      | Manual       | Manual               | Django Filter          |
| Busca        | Manual       | Manual               | SearchFilter           |
| Ordenação    | Manual       | Manual               | OrderingFilter         |
| Paginação    | Manual       | Manual               | `PageNumberPagination` |
| Documentação | Configuração | Automática           | OpenAPI/Swagger        |
| CRUD         | Manual       | Relativamente manual | ViewSet                |

A tabela poderá ser ampliada conforme novos conceitos forem introduzidos.

A comparação deverá acontecer ao longo da disciplina, especialmente durante a sequência:

```text
Express → conceito
FastAPI → mesmo conceito
Comparação
```

e posteriormente:

```text
Django → abstração equivalente
```

O objetivo não é declarar uma tecnologia como "melhor", mas mostrar:

> **qual problema cada abstração resolve e quanto trabalho ela retira do desenvolvedor.**

---

# 22. Vue.js

O Vue.js não deverá ser introduzido no início da construção das APIs.

Primeiro os alunos deverão compreender:

```text
Cliente
   ↓
HTTP Request
   ↓
API
   ↓
HTTP Response
   ↓
Cliente
```

Esse fluxo poderá ser explorado utilizando:

* navegador;
* Swagger;
* Postman/Insomnia;
* `curl`.

Somente depois que o aluno compreender esse mecanismo o Vue.js será introduzido.

---

# 23. Momento do Vue.js na sequência da disciplina

O Vue.js será introduzido **somente depois da consolidação das APIs**.

A sequência geral será:

```text
Fundamentos da Web
        ↓
HTTP e REST
        ↓
API-base
        ↓
Express: conceito
        ↓
FastAPI: mesmo conceito
        ↓
Comparação
        ↓
Próximo conceito
        ↓
Express: implementação
        ↓
FastAPI: implementação
        ↓
Comparação
        ↓
...
        ↓
Django REST Framework
        ↓
Evolução da API
        ↓
Consumo da API
        ↓
Vue.js
        ↓
Integração Vue + API
        ↓
Projeto individual
```

O Vue entra, portanto, **depois da consolidação do backend e antes do projeto individual**.

---

# 24. Objetivo do Vue na disciplina

O objetivo não é transformar Desenvolvimento Web 2 em um curso completo de Vue.js.

O objetivo é ensinar a integração entre frontend e backend:

```text
Frontend
   │
   │ HTTP / JSON
   ▼
REST API
   │
   ▼
Banco de dados
```

Na etapa comum, espera-se que os alunos sejam capazes de construir uma interface capaz de:

* listar;
* cadastrar;
* editar;
* excluir;
* pesquisar ou filtrar;
* navegar entre páginas de resultados.

A partir daí, os projetos individuais poderão aprofundar o frontend conforme suas necessidades.

---

# 25. Sequência pedagógica geral

A sequência geral da disciplina será organizada por **conceitos e etapas de aprendizagem**, com Express e FastAPI sendo desenvolvidos de forma alternada.

## Etapa 1 — Fundamentos

Conteúdos:

* HTTP;
* cliente e servidor;
* requisição e resposta;
* métodos HTTP;
* códigos de status;
* JSON;
* REST;
* CRUD.

---

## Etapa 2 — API-base

Definição e construção progressiva da API de Produtos.

Modelo inicial:

```text
Produto
- id
- nome
- preco
```

Contrato inicial:

```text
GET /api/produtos/
GET /api/produtos/{id}/
POST /api/produtos/
PUT /api/produtos/{id}/
DELETE /api/produtos/{id}/
```

Também fazem parte do contrato-base:

```text
nome:
- obrigatório
- trim
- 2 a 100 caracteres

preco:
- obrigatório
- maior que zero
- máximo de 2 casas decimais
```

As respostas de sucesso de recursos individuais utilizarão JSON direto.

As listagens utilizarão o contrato de paginação definido na seção 7.5.

Os erros utilizarão o campo:

```json
{
    "detail": "..."
}
```

ou, para validações estruturadas:

```json
{
    "detail": {
        "campo": "mensagem"
    }
}
```

O `PUT` será tratado como atualização completa.

O `PATCH` ficará fora da etapa inicial.

A paginação utilizará:

```text
page
page_size
```

com:

```text
page_size padrão: 10
page_size máximo: 100
```

e resposta:

```json
{
    "page": 1,
    "page_size": 10,
    "total_pages": 5,
    "results": []
}
```

---

## Etapa 3 — Express e FastAPI: construção progressiva

Express e FastAPI não serão tratados como duas etapas completamente separadas.

A metodologia será:

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

Por exemplo:

```text
GET lista
   ↓
Express
   ↓
FastAPI
   ↓
GET por ID
   ↓
Express
   ↓
FastAPI
   ↓
POST
   ↓
Express
   ↓
FastAPI
```

O mesmo princípio será utilizado para:

* CRUD;
* validação;
* filtros;
* busca;
* ordenação;
* paginação;
* persistência.

---

## Etapa 4 — Consolidação Express × FastAPI

Depois de percorrer os principais conceitos, será realizada uma comparação mais sistemática.

Perguntas importantes:

* O que tivemos que implementar manualmente?
* O que o framework forneceu?
* Como a validação mudou?
* Como as rotas são declaradas?
* Como os parâmetros são tratados?
* Como a documentação é gerada?
* Como a persistência é implementada?
* Como a paginação é implementada?
* Onde existe mais abstração?
* O que ficou mais explícito em cada tecnologia?

---

## Etapa 5 — Django REST Framework

Construção da API utilizando DRF.

Foco:

* Models;
* ORM;
* migrations;
* Serializers;
* ViewSets;
* Routers;
* filtros;
* busca;
* ordenação;
* paginação;
* OpenAPI;
* Swagger.

O Django começará comparável à API-base, mas não precisa permanecer artificialmente limitado a ela.

---

## Etapa 6 — Evolução da API

Somente depois de consolidada a API básica poderão ser introduzidos recursos mais sofisticados, especialmente no Django:

* descrição;
* estoque;
* timestamps;
* Categoria;
* relacionamentos;
* filtros mais avançados.

Essa etapa deverá ser controlada para não comprometer o tempo destinado aos projetos.

---

## Etapa 7 — Consumo da API

Antes do Vue, os alunos deverão consumir a API utilizando ferramentas simples:

* Swagger;
* Postman/Insomnia;
* navegador;
* `curl`.

O objetivo é reforçar que uma API pode ser consumida independentemente do frontend.

Também deverá ser demonstrado o consumo de uma coleção paginada, incluindo:

```text
page
page_size
total_pages
results
```

O aluno deverá compreender que o frontend precisa interpretar os metadados da paginação para construir a navegação entre páginas.

---

## Etapa 8 — Vue.js

O Vue será utilizado para construir um cliente da API.

Primeiro:

```text
GET → lista
```

Depois:

```text
GET
POST
PUT
DELETE
```

Finalmente, poderão ser adicionados:

```text
filtros
busca
ordenação
paginação
```

A paginação no frontend deverá utilizar o mesmo contrato definido para as APIs:

```text
page
page_size
total_pages
results
```

---

## Etapa 9 — Projeto individual

Depois da etapa comum, cada aluno ou equipe deverá aplicar os conhecimentos em um projeto próprio.

O projeto poderá utilizar outro domínio, por exemplo:

* livros;
* filmes;
* animais;
* eventos;
* jogos;
* cursos;
* produtos;
* serviços.

O objetivo é que o aluno deixe de reproduzir exemplos e passe a tomar decisões próprias de modelagem, API e interface.

---

# 26. Estratégia de tempo

A etapa introdutória deve ser mantida deliberadamente enxuta.

A prioridade é que o aluno compreenda profundamente os conceitos fundamentais antes de partir para o projeto individual.

A estratégia Express → FastAPI por conceito também contribui para o controle do tempo.

Em vez de:

```text
Express completo
      ↓
FastAPI completo
```

a disciplina adotará:

```text
Conceito 1
   ↓
Express
   ↓
FastAPI
   ↓
Comparação
   ↓
Conceito 2
   ↓
Express
   ↓
FastAPI
   ↓
Comparação
   ↓
...
```

Isso reduz a sensação de repetição e permite que a comparação aconteça no momento em que o aluno ainda está pensando sobre o conceito.

A sequência desejada é:

```text
Conceitos
   ↓
API simples
   ↓
CRUD
   ↓
Express ↔ FastAPI por conceito
   ↓
Django REST Framework
   ↓
Vue
   ↓
Projeto individual
```

Não devemos continuar adicionando funcionalidades apenas para demonstrar recursos de um framework quando os alunos já tiverem adquirido os conceitos necessários.

O tempo economizado nesta etapa será utilizado no desenvolvimento dos projetos individuais.

---

# 27. Alterações necessárias nas aplicações atuais

## FastAPI

A aplicação atual deverá ser revisada para:

* adotar `/api/produtos/`;
* manter inicialmente apenas `id`, `nome` e `preco`;
* começar com dados em memória;
* posteriormente persistir em JSON;
* revisar filtros;
* implementar busca;
* revisar ordenação;
* implementar paginação;
* utilizar `page` e `page_size`;
* utilizar `page_size` padrão de 10;
* limitar `page_size` a 100;
* retornar `page`, `page_size`, `total_pages` e `results` nas listagens;
* padronizar respostas de sucesso;
* padronizar respostas de erro;
* utilizar os códigos HTTP definidos;
* manter as regras de validação;
* manter documentação automática;
* preservar o caráter relativamente manual da implementação.

Não é necessário adicionar Categoria.

A implementação deverá ser alinhada progressivamente às etapas correspondentes já existentes no Express.

---

## Express

A aplicação atual deverá ser revisada para:

* adotar `/api/produtos/`;
* manter inicialmente apenas `id`, `nome` e `preco`;
* começar com dados em memória;
* posteriormente persistir em JSON;
* revisar filtros;
* implementar busca;
* revisar ordenação;
* implementar paginação;
* utilizar `page` e `page_size`;
* utilizar `page_size` padrão de 10;
* limitar `page_size` a 100;
* retornar `page`, `page_size`, `total_pages` e `results` nas listagens;
* padronizar respostas de sucesso;
* padronizar respostas de erro;
* utilizar os códigos HTTP definidos;
* manter as regras de validação;
* manter a implementação explícita das regras;
* documentar a API.

Não é necessário adicionar Categoria.

A implementação existente deverá ser preservada sempre que fizer sentido pedagogicamente, ajustando-a ao contrato comum.

---

## Django

A aplicação atual deverá ser revisada conceitualmente para definir a forma mais adequada de iniciar a API.

A recomendação atual é que o Django também comece com:

```text
Produto
- id
- nome
- preco
```

A paginação deverá utilizar o mecanismo de paginação baseado em número de página do Django REST Framework.

A configuração pedagógica de referência será equivalente a:

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS':
        'app.pagination.CustomPagination',

    'PAGE_SIZE': 10,
}
```

Com uma classe de paginação baseada em:

```python
from rest_framework import pagination
from rest_framework.response import Response


class CustomPagination(pagination.PageNumberPagination):
    page_size = 10
    page_size_query_param = 'page_size'
    max_page_size = 100

    def get_paginated_response(self, data):
        return Response({
            'page': self.page.number,
            'page_size': self.page.paginator.per_page,
            'total_pages': self.page.paginator.num_pages,
            'results': data,
        })
```

A configuração poderá conter também as demais opções necessárias da aplicação, como autenticação, permissões e documentação.

O importante para o contrato pedagógico é que a resposta observada pelo cliente seja:

```json
{
    "page": 1,
    "page_size": 10,
    "total_pages": 5,
    "results": []
}
```

Depois poderá evoluir para:

```text
Produto
- id
- nome
- descricao
- estoque
- preco
- criado_em
- atualizado_em
- categoria
```

A estrutura atual com `Categoria` não precisa ser apagada neste momento.

Ela poderá ser utilizada posteriormente como uma evolução didática.

O Django, portanto, não deverá ser artificialmente limitado apenas para reproduzir todas as decisões tomadas para Express e FastAPI.

---

# 28. Documentação

A documentação da disciplina deverá destacar:

> **mesma API, diferentes implementações.**

Não devemos apresentar Express, FastAPI e Django como três projetos completamente independentes.

Cada tecnologia deverá responder à mesma pergunta:

> **Como implementar este requisito nesta tecnologia?**

No caso de Express e FastAPI, essa pergunta será respondida de forma alternada ao longo das etapas.

O README será único e terá capítulos numerados sequencialmente.

As seções internas poderão organizar os detalhes de cada tecnologia e conceito.

Os arquivos de código, por outro lado, continuarão sendo progressivos e poderão possuir suas próprias versões correspondentes a cada etapa.

Essa separação é importante:

```text
README
→ organização conceitual e sequencial da disciplina

Arquivos de código
→ evolução prática de cada implementação
```

O README não deverá ser fragmentado em um arquivo por aula.

Os arquivos de código não precisam reproduzir a numeração dos capítulos do README.

---

# 29. Decisões fechadas para a implementação

As seguintes decisões estão **definitivamente estabelecidas** e deverão ser consideradas como referência para os próximos trabalhos de implementação.

## 29.1 Modelo inicial

```text
Produto
├── id
├── nome
└── preco
```

---

## 29.2 URL-base

```text
/api/produtos/
```

---

## 29.3 Operações

```text
GET    /api/produtos/
GET    /api/produtos/{id}/
POST   /api/produtos/
PUT    /api/produtos/{id}/
DELETE /api/produtos/{id}/
```

---

## 29.4 PUT

O `PUT` representa **atualização completa** do recurso.

O `PATCH` não faz parte da etapa inicial.

---

## 29.5 Validação

### Nome

```text
obrigatório
string
trim
mínimo: 2 caracteres
máximo: 100 caracteres
```

### Preço

```text
obrigatório
numérico
maior que zero
máximo de 2 casas decimais
```

---

## 29.6 Respostas de sucesso

Recursos individuais utilizarão JSON direto, sem envelopes genéricos:

```json
{
    "id": 1,
    "nome": "Notebook",
    "preco": 3500.00
}
```

Listagens utilizarão o contrato de paginação:

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

Não serão utilizados envelopes genéricos como:

```json
{
    "message": "Produto criado com sucesso",
    "dados": {}
}
```

---

## 29.7 Respostas de erro

Erro simples:

```json
{
    "detail": "Produto não encontrado."
}
```

Erro de validação:

```json
{
    "detail": {
        "nome": "O nome deve possuir entre 2 e 100 caracteres.",
        "preco": "O preço deve ser maior que zero."
    }
}
```

---

## 29.8 Códigos HTTP

```text
GET    → 200
POST   → 201
PUT    → 200
DELETE → 204
Erro de validação → 400
Não encontrado   → 404
```

---

## 29.9 Paginação

A paginação seguirá o padrão de **número da página**, inspirado no Django REST Framework.

Parâmetros:

```text
page
page_size
```

Exemplos:

```http
GET /api/produtos/?page=1
```

```http
GET /api/produtos/?page=2&page_size=20
```

Configuração conceitual:

```text
page_size padrão → 10
page_size máximo  → 100
```

Contrato da resposta:

```json
{
    "page": 1,
    "page_size": 10,
    "total_pages": 5,
    "results": []
}
```

Campos:

```text
page
→ página atual

page_size
→ quantidade de itens solicitada para a página

total_pages
→ quantidade total de páginas

results
→ resultados da página atual
```

A paginação deverá funcionar em conjunto com:

```text
filtros
busca
ordenação
```

A ordem conceitual será:

```text
dados
  ↓
filtros
  ↓
busca
  ↓
ordenação
  ↓
paginação
  ↓
resposta
```

No Express e FastAPI a paginação será implementada manualmente.

No Django REST Framework será utilizada uma implementação baseada em `PageNumberPagination`, com `page_size_query_param = 'page_size'` e `max_page_size = 100`.

O contrato externo deverá ser equivalente nas três tecnologias.

---

## 29.10 Arquivos progressivos

A convenção será:

```text
NN-descricao
```

com numeração correspondente entre Express e FastAPI quando representarem o mesmo conceito.

Exemplo:

```text
07-validacao.js
07-validacao.py
```

---

## 29.11 Arquivo da API completa

A versão completa de cada sequência deverá utilizar:

```text
NN-api-completa
```

Exemplo:

```text
12-api-completa.js
12-api-completa.py
```

A convenção `-final` não será utilizada.

A convenção `-completo` também não será utilizada.

---

## 29.12 README

Haverá:

```text
1 README único
```

com:

```text
capítulos numerados sequencialmente
        ↓
seções internas
        ↓
organização conceitual
```

Não haverá um README independente para cada aula.

---

## 29.13 Metodologia Express → FastAPI

A ordem de desenvolvimento será:

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

Não será adotada a estratégia:

```text
todo Express
   ↓
todo FastAPI
```

---

# 30. Próximo passo

O próximo passo **não será terminar primeiro o Express e depois começar o FastAPI**.

O próximo passo será revisar as implementações existentes e estabelecer um **mapa de correspondência entre as etapas atuais de Express e FastAPI**, preservando o que já foi produzido.

Esse trabalho deverá seguir esta ordem:

1. revisar os arquivos existentes do Express;
2. revisar os arquivos existentes do FastAPI;
3. identificar até onde cada implementação já chegou;
4. mapear as etapas equivalentes entre as duas tecnologias;
5. identificar lacunas e diferenças de sequência;
6. preservar a evolução didática que já foi produzida;
7. reorganizar as etapas para a sequência **Express → FastAPI por conceito**;
8. ajustar ambas ao contrato comum;
9. definir as etapas que ainda precisam ser implementadas;
10. produzir ou ajustar os arquivos progressivos;
11. criar a versão `NN-api-completa` de cada implementação;
12. revisar o Django;
13. definir a etapa de evolução do Django;
14. estruturar o README único com capítulos numerados;
15. preparar o conteúdo de consumo da API, incluindo o consumo de listas paginadas;
16. introduzir Vue.js;
17. iniciar os projetos individuais.

O princípio geral passa a ser:

> **primeiro compreender o conceito, depois implementá-lo no Express, em seguida implementar o equivalente no FastAPI, comparar as soluções e somente então avançar para o próximo conceito.**

E, em relação à documentação:

> **um único README organiza a sequência didática; os arquivos de código permanecem evolutivos e registram as diferentes etapas de implementação.**

A metodologia completa pode ser representada assim:

```text
FUNDAMENTOS
    ↓
API-BASE
    ↓
CONCEITO
    ↓
EXPRESS
    ↓
FASTAPI
    ↓
COMPARAÇÃO
    ↓
PRÓXIMO CONCEITO
    ↓
EXPRESS
    ↓
FASTAPI
    ↓
COMPARAÇÃO
    ↓
...
    ↓
DJANGO REST FRAMEWORK
    ↓
EVOLUÇÃO DA API
    ↓
CONSUMO
    ↓
VUE.JS
    ↓
PROJETO INDIVIDUAL
```
