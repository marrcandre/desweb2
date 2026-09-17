# Projeto Final — Desenvolvimento Web 2

## 1. Objetivo

Desenvolver uma aplicação Web própria que utilize os conceitos estudados em Desenvolvimento Web 2. Você deverá construir uma aplicação baseada em uma API REST, com dados persistidos e regras coerentes com o domínio escolhido.

## 2. Proposta

Escolha um domínio de sua preferência e desenvolva uma aplicação para esse domínio. A aplicação poderá tratar, por exemplo, de livros, eventos, cursos, jogos, tarefas ou outro assunto aprovado pelo professor.

O trabalho poderá ser realizado individualmente ou em duplas.

O projeto deverá aplicar os conceitos trabalhados na disciplina, incluindo:

- HTTP, cliente, servidor, requisição e resposta;
- métodos HTTP e códigos de status;
- JSON e organização REST;
- operações de CRUD;
- construção e consumo de APIs;
- persistência de dados;
- ORM, quando Django/DRF for utilizado;
- validação e regras de negócio;
- filtros, busca, ordenação e paginação;
- documentação de API;
- integração entre backend e frontend, quando o requisito de frontend for realizado.

Os requisitos 1 a 4 são obrigatórios e correspondem à implementação técnica da aplicação. O requisito 5, apresentação do projeto, também é obrigatório. Os requisitos 1 a 4 totalizam 5,0 pontos, e a apresentação completa os 6,0 pontos fundamentais. Os requisitos 6 a 9 são recursos de evolução opcionais, e cada um vale 1,0 ponto adicional.

## 3. Requisitos obrigatórios

Os requisitos técnicos fundamentais totalizam 5,0 pontos. Com a apresentação obrigatória, os requisitos fundamentais totalizam 6,0 pontos. Eles não podem ser substituídos por funcionalidades opcionais.

### 3.1 API REST — 1,5 ponto

Crie uma API REST funcional para o domínio escolhido.

A API deverá:

- possuir pelo menos um recurso principal;
- implementar as operações fundamentais de CRUD:
  - `GET` da coleção;
  - `GET` de um recurso individual;
  - `POST`;
  - `PUT`;
  - `DELETE`;
- utilizar rotas organizadas de maneira coerente com os princípios REST trabalhados na disciplina;
- receber e retornar dados em JSON quando aplicável;
- utilizar códigos HTTP adequados para sucesso, erro de validação e recurso não encontrado.

O `PUT` deverá representar a atualização completa do recurso, conforme trabalhado nas aulas. A documentação deverá informar o formato esperado das requisições e respostas.

Você poderá utilizar Express, FastAPI ou Django REST Framework, conforme os conteúdos da disciplina. O uso de outra tecnologia de backend poderá ser autorizado previamente pelo professor, desde que permita demonstrar os mesmos conceitos e requisitos previstos neste projeto.

### 3.2 Persistência de dados — 1,0 ponto

Os dados não podem permanecer somente em memória durante a execução da aplicação.

A aplicação deverá:

- utilizar um banco de dados;
- definir adequadamente as entidades persistidas;
- permitir que os dados continuem disponíveis após o encerramento e o reinício do servidor;
- incluir as configurações e instruções necessárias para criar ou preparar o banco de dados.

Quando Django/DRF for utilizado, deverá ser usado o ORM do Django. O projeto deverá possuir `models` e `migrations` adequadamente definidos.

### 3.3 Validação e regra de negócio — 1,5 ponto

A aplicação deverá validar os dados recebidos pela API e tratar entradas inválidas com respostas adequadas.

Além do CRUD, deverá existir pelo menos uma regra de negócio relacionada ao domínio escolhido. Essa regra deverá:

- fazer sentido para a aplicação;
- estar implementada no backend;
- ser verificável por meio da API e/ou da aplicação;
- produzir comportamento diferente de simplesmente criar, listar, atualizar ou excluir registros.

Exemplos: impedir o empréstimo de um livro indisponível, impedir a inscrição em um evento lotado ou impedir a criação de uma reserva em conflito de horário.

### 3.4 Consulta dos dados — 1,0 ponto

A API deverá permitir consultar os dados além do simples `GET` da coleção.

Deverá possuir, de forma funcional e demonstrável:

- filtro;
- busca textual;
- ordenação;
- paginação.

Esses recursos deverão ser documentados, possuir parâmetros compreensíveis e funcionar com os dados persistidos da aplicação.

### 3.5 Apresentação do projeto — 1,0 ponto

A apresentação do projeto é obrigatória e será realizada em sala de aula, para toda a turma, após a data de entrega.

Durante a apresentação, você deverá demonstrar e explicar:

- o domínio escolhido;
- as principais entidades e models;
- o funcionamento da API;
- as operações principais;
- a persistência;
- a validação;
- a regra de negócio;
- o filtro, a busca textual, a ordenação e a paginação;
- os recursos opcionais implementados, quando houver.

Quando o trabalho for realizado em dupla, os dois integrantes deverão participar da apresentação e demonstrar individualmente domínio do projeto desenvolvido.

Ambos deverão ser capazes de explicar o projeto e demonstrar individualmente domínio suficiente sobre o trabalho. Portanto, a nota da apresentação poderá ser diferente entre os integrantes da dupla, mesmo que o projeto seja o mesmo.

## 4. Recursos de evolução

Os recursos a seguir correspondem aos requisitos 6 a 9 e são opcionais. Cada requisito realizado vale 1,0 ponto, até o limite de 4,0 pontos adicionais. Eles não compensam a ausência de qualquer requisito obrigatório.

### 4.1 Frontend — 1,0 ponto

Desenvolva um frontend utilizando Vue.js ou outra tecnologia previamente aprovada pelo professor.

O frontend deverá:

- consumir efetivamente a API desenvolvida no projeto;
- permitir ao usuário realizar operações relevantes sobre os dados;
- apresentar os resultados das operações e os erros de forma compreensível;
- funcionar como parte da aplicação, e não apenas como uma página estática ou um mock da API.

### 4.2 Modelagem com 3 ou mais models/tabelas — 1,0 ponto

A aplicação deverá possuir **3 ou mais models/tabelas** que representem de forma coerente o domínio da aplicação.

Os models/tabelas deverão:

- representar necessidades reais do domínio;
- possuir nomes e campos coerentes;
- ser utilizados por alguma operação da aplicação;
- estar documentados.

Não serão considerados para este requisito models/tabelas artificiais criados somente para atingir a quantidade mínima. Não é necessário que todos os models/tabelas tenham relacionamento direto entre si.

### 4.3 Publicação — 1,0 ponto

Publique a aplicação para acesso externo pela Internet.

Para cumprir este requisito:

- a API deverá estar acessível por uma URL pública;
- se o frontend for desenvolvido, ele também deverá estar acessível por uma URL pública;
- as URLs deverão ser informadas na entrega;
- a aplicação deverá continuar funcionando no momento da avaliação.

Caso frontend e backend estejam em plataformas diferentes, as duas URLs deverão estar disponíveis e integradas. A plataforma de publicação fica a critério do aluno, desde que o acesso externo seja possível.

### 4.4 Autenticação — 1,0 ponto

Implemente autenticação na API.

Para cumprir este requisito:

- deverá existir um mecanismo real de login e autenticação de usuários;
- determinados endpoints ou operações poderão ser públicos;
- determinados endpoints ou operações deverão exigir que o usuário esteja autenticado;
- a API deverá identificar o usuário autenticado nas requisições protegidas;
- deverá ser possível demonstrar uma requisição autenticada e o acesso a pelo menos um recurso protegido.

O simples cadastro de usuários, sem um mecanismo real de login e autenticação, não caracteriza autenticação. Não é necessário implementar perfis, roles, níveis diferentes de acesso ou permissões administrativas.

## 5. Sugestões de aplicações

As ideias abaixo servem apenas como inspiração. Você não precisa escolher uma delas. Em qualquer caso, escolha um domínio que permita criar regras de negócio e, se desejar, uma modelagem com 3 ou mais models/tabelas.

<details>
<summary>Exemplos de entidades e regras de negócio</summary>

### Biblioteca

- Possíveis entidades: `Livro`, `Autor`, `Categoria`, `Usuario` e `Emprestimo`.
- Regra possível: um livro indisponível não pode ser emprestado novamente.

### Catálogo de filmes e séries

- Possíveis entidades: `Filme`, `Serie`, `Genero`, `Ator`, `Avaliacao` e `Usuario`.
- Regra possível: uma avaliação só pode ser registrada uma vez por usuário para cada título.

### Gerenciamento de eventos

- Possíveis entidades: `Evento`, `Local`, `Organizador`, `Participante` e `Inscricao`.
- Regra possível: não permitir inscrições quando a capacidade do evento estiver esgotada.

### Cursos

- Possíveis entidades: `Curso`, `Modulo`, `Aula`, `Aluno`, `Instrutor` e `Matricula`.
- Regra possível: somente alunos matriculados podem registrar progresso nas aulas.

### Animais e adoção

- Possíveis entidades: `Animal`, `Especie`, `Abrigo`, `Pessoa` e `Adocao`.
- Regra possível: um animal já adotado não pode receber uma nova adoção.

### Jogos

- Possíveis entidades: `Jogo`, `Genero`, `Plataforma`, `Desenvolvedora`, `Usuario` e `Avaliacao`.
- Regra possível: uma avaliação deve estar associada a um jogo que o usuário tenha registrado como jogado.

### Restaurante

- Possíveis entidades: `Prato`, `Categoria`, `Ingrediente`, `Mesa`, `Pedido` e `ItemPedido`.
- Regra possível: não permitir adicionar ao pedido um prato indisponível.

### Academia

- Possíveis entidades: `Aluno`, `Plano`, `Professor`, `Treino`, `Exercicio` e `Aula`.
- Regra possível: um aluno não pode reservar duas aulas no mesmo horário.

### Viagens

- Possíveis entidades: `Destino`, `Viagem`, `Passageiro`, `Reserva`, `Pagamento` e `Transporte`.
- Regra possível: não permitir reservas acima da quantidade de vagas disponíveis.

### Música

- Possíveis entidades: `Musica`, `Album`, `Artista`, `Genero`, `Playlist` e `Usuario`.
- Regra possível: uma música não pode ser adicionada duas vezes à mesma playlist.

### Estoque

- Possíveis entidades: `Produto`, `Categoria`, `Fornecedor`, `Entrada`, `Saida` e `Usuario`.
- Regra possível: não permitir uma saída maior que a quantidade disponível em estoque.

### Tarefas

- Possíveis entidades: `Projeto`, `Tarefa`, `Usuario`, `Equipe`, `Etiqueta` e `Comentario`.
- Regra possível: somente usuários atribuídos ao projeto podem alterar suas tarefas.

### Biblioteca de jogos

- Possíveis entidades: `Jogo`, `Plataforma`, `Genero`, `Colecao`, `Usuario` e `Emprestimo`.
- Regra possível: um jogo emprestado não pode ser incluído em outro empréstimo simultâneo.

### Agenda

- Possíveis entidades: `Contato`, `Evento`, `Local`, `Categoria`, `Usuario` e `Lembrete`.
- Regra possível: impedir eventos com conflito de horário para o mesmo usuário.

### Gerenciamento de projetos

- Possíveis entidades: `Projeto`, `Equipe`, `Membro`, `Tarefa`, `Status` e `Comentario`.
- Regra possível: uma tarefa concluída não pode voltar para um estado anterior sem uma justificativa registrada.

</details>

## 6. Requisitos técnicos

O projeto deverá ser entregue com:

- código-fonte versionado em Git;
- um `README` próprio do projeto;
- instruções para instalação, configuração e execução;
- descrição do domínio, das principais entidades e das regras de negócio;
- documentação da API, incluindo endpoints, métodos, parâmetros, corpos esperados e respostas;
- informações necessárias para testar a aplicação;
- URL da aplicação publicada, quando o requisito de publicação for realizado;
- URL da API, quando aplicável.

A documentação da API poderá utilizar os recursos disponíveis na tecnologia escolhida, como Swagger/OpenAPI, quando aplicável.

O uso de documentação, bibliotecas, frameworks, exemplos, ferramentas de desenvolvimento, ferramentas de IA ou outros recursos de apoio é permitido. Entretanto, os alunos deverão compreender o projeto entregue e ser capazes de explicar suas principais decisões, funcionamento e código durante a avaliação.

Não são exigidos microserviços, Docker, Kubernetes, CI/CD, arquitetura excessivamente sofisticada ou testes extensivos. O foco é demonstrar os conceitos de desenvolvimento Web trabalhados na disciplina.

## 7. Entrega

Até **19/11/2026**, entregue:

- pelo SIGAA, enviando o link do repositório GitHub do projeto;
- o `README` com as instruções e a documentação solicitadas;
- a URL da API, quando aplicável;
- a URL do frontend, caso o requisito de frontend tenha sido realizado;
- as credenciais de teste, caso exista autenticação;
- outras informações necessárias para executar e avaliar a aplicação.

As URLs e as credenciais devem estar válidas no momento da avaliação. Não publique senhas reais, chaves privadas ou outros segredos no repositório.

Após a data de entrega, serão realizadas apresentações dos projetos em sala de aula, para toda a turma. A apresentação fará parte da avaliação da aplicação entregue.

## 8. Critérios de avaliação

A correção considerará os pontos previstos para cada requisito. Dentro desses pontos, serão observados:

- funcionamento das operações e dos fluxos demonstrados;
- coerência da modelagem com o domínio escolhido;
- qualidade e organização das rotas, respostas e códigos HTTP;
- validação dos dados e tratamento de erros;
- implementação efetiva da regra de negócio;
- funcionamento de filtro, busca textual, ordenação e paginação;
- organização e legibilidade do código;
- clareza e completude da documentação;
- integração entre frontend e backend, quando aplicável;
- coerência dos models/tabelas, quando o requisito de modelagem for realizado;
- funcionamento da autenticação, quando o requisito correspondente for realizado;
- capacidade de demonstrar e explicar o projeto na apresentação obrigatória;
- disponibilidade e funcionamento das URLs, quando o requisito de publicação for realizado.

Esses critérios não criam pontos adicionais. Eles serão utilizados dentro da pontuação dos requisitos correspondentes.

## 9. Tabela de pontuação

### Requisitos fundamentais

| Requisito | Obrigatório | Pontos |
|---|---:|---:|
| 1. API REST | Sim | 1,5 |
| 2. Persistência de dados | Sim | 1,0 |
| 3. Validação e regra de negócio | Sim | 1,5 |
| 4. Consulta dos dados | Sim | 1,0 |
| 5. Apresentação do projeto | Sim | 1,0 |
| **Subtotal** |  | **6,0** |

### Recursos de evolução

| Requisito | Obrigatório | Pontos |
|---|---:|---:|
| 6. Frontend | Não | 1,0 |
| 7. Modelagem com 3 ou mais models/tabelas | Não | 1,0 |
| 8. Publicação | Não | 1,0 |
| 9. Autenticação | Não | 1,0 |
| **Subtotal** |  | **4,0** |

| Resultado | Pontos |
|---|---:|
| **Nota máxima** | **10,0** |

Os requisitos 1 a 4 são obrigatórios e representam a implementação técnica essencial da disciplina. O requisito 5, apresentação do projeto, também é obrigatório. Os requisitos 1 a 4 totalizam 5,0 pontos, e os requisitos fundamentais totalizam 6,0 pontos com a apresentação. Os requisitos 6 a 9 são opcionais, valem 1,0 ponto cada e não podem ser usados para compensar a ausência de um requisito fundamental.

## 10. Checklist antes da entrega

- [ ] Escolhi um domínio próprio, diferente da simples reprodução da API de Produtos.
- [ ] Implementei `GET` da coleção, `GET` individual, `POST`, `PUT` e `DELETE`.
- [ ] Utilizei códigos HTTP adequados e respostas em JSON quando aplicável.
- [ ] Os dados estão persistidos em um banco de dados.
- [ ] Defini e implementei validações no backend.
- [ ] Implementei pelo menos uma regra de negócio além do CRUD.
- [ ] Implementei filtro, busca textual, ordenação e paginação.
- [ ] Documentei a API e as instruções de execução.
- [ ] Versionei o código em um repositório Git.
- [ ] Informei todas as URLs necessárias para avaliação.
- [ ] Publiquei a aplicação, caso tenha realizado o requisito de publicação.
- [ ] Criei um frontend integrado à API, caso tenha realizado esse requisito.
- [ ] Implementei 3 ou mais models/tabelas relevantes para o domínio, sem contar tabelas automáticas ou internas do framework, caso tenha realizado esse requisito.
- [ ] Implementei e testei login, autenticação e acesso a pelo menos um recurso protegido, caso tenha realizado esse requisito.
- [ ] Preparei a demonstração e a explicação do projeto para a apresentação obrigatória.
- [ ] Realizei a entrega pelo SIGAA, enviando o link do repositório GitHub do projeto.
- [ ] Estou preparado para apresentar o projeto em sala de aula, para toda a turma, após a data de entrega.
- [ ] Removi do repositório credenciais, senhas e chaves privadas.
- [ ] Testei a aplicação após instalar e configurar o projeto seguindo o `README`.