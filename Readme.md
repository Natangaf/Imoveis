# Imóveis - TypeORM com Relacionamentos

## Introdução

O projeto KImóveis é uma API desenvolvida em TypeScript para o gerenciamento de uma imobiliária. A aplicação permite o cadastro de imóveis e usuários interessados na aquisição de propriedades. Além disso, é possível realizar agendamentos e consultar horários de visitas às propriedades disponíveis no banco de dados da imobiliária.

A seguir, estão listados os endpoints disponíveis na API, bem como as regras e especificações para cada rota:

## Endpoints

### POST /users

- Criação de usuário com os seguintes dados:
  - name: string, máximo de 45 caracteres e obrigatório
  - email: string, máximo de 45 caracteres, obrigatório e único.
  - password: string, máximo de 120 caracteres e obrigatório (será armazenada como hash).
  - admin: boolean, com valor padrão false.
  - createdAt: data de criação gerada automaticamente.
  - updatedAt: data de atualização gerada automaticamente.
  - deletedAt: data de exclusão gerada automaticamente em caso de soft delete.

### GET /users

- Lista todos os usuários (com exceção da senha) e pode ser acessada apenas por administradores.

### PATCH /users/:id

- Atualiza os dados do usuário.
- Não é possível atualizar os campos id e admin.
- Apenas administradores podem atualizar qualquer usuário, enquanto usuários não-administradores podem atualizar apenas o seu próprio usuário.

### DELETE /users/:id

- Realiza um soft delete do usuário (não remove fisicamente do banco de dados).
- Apenas administradores podem realizar o soft delete de usuários.

### POST /login

- Rota de login que recebe email e senha para autenticação.
- Valida se o usuário existe e se a senha está correta.
- Não é possível fazer login de um usuário marcado como deletado.

### POST /categories

- Criação de categoria com os seguintes dados:
  - name: string, máximo de 45 caracteres, obrigatório e único.
- Não podem ser cadastradas duas categorias com o mesmo nome.
- A rota pode ser acessada apenas por usuários administradores.

### GET /categories

- Lista todas as categorias e pode ser acessada por qualquer usuário.

### GET /categories/:id/realEstate

- Lista todos os imóveis que pertencem a uma categoria específica.
- Pode ser acessada por qualquer usuário.

### POST /realEstate

- Criação de um imóvel com os seguintes dados:
  - value: decimal, obrigatório e 0 por padrão.
  - size: inteiro e obrigatório.
  - address: objeto contendo:
    - street: string, máximo de 45 caracteres e obrigatório.
    - zipCode: string, máximo de 8 caracteres e obrigatório.
    - number: string, máximo de 7 caracteres e opcional.
    - city: string, máximo de 20 caracteres e obrigatório.
    - state: string, máximo de 2 caracteres e obrigatório.
  - categoryId: inteiro e obrigatório.
  - sold: boolean, gerado automaticamente com valor padrão false.
  - createdAt: data de criação gerada automaticamente.
  - updatedAt: data de atualização gerada automaticamente.
- Não podem ser cadastrados dois imóveis com o mesmo endereço.
- A rota pode ser acessada apenas por administradores.

### GET /realEstate

- Lista todos os imóveis e pode ser acessada por qualquer usuário.

### POST /schedules

- Responsável pelo agendamento de uma visita a um imóvel com os seguintes dados:
  - date: string da data de agendamento da visita ao imóvel, no formato americano AAAA-MM-DD.
  - hour: string do horário de agendamento da visita ao imóvel, no formato HH:MM.
  - realEstateId: inteiro e obrigatório.
- Não é possível o mesmo usuário agendar uma visita a 2 imóveis diferentes com a mesma data e hora.
- Não é possível agendar uma visita a um imóvel com a mesma data e hora.
- Só é possível agendar uma visita durante horário comercial (08:00 às 18:00).
- Só é possível agendar uma visita durante dias úteis (segunda à sexta).
- A rota pode ser acessada tanto por usuários comuns quanto administradores.

### GET /schedules/realEstate/:id

- Lista todos os agendamentos de um imóvel específico.
- Pode ser acessada apenas por administradores.

## Requisitos do Serviço

- O código deve estar em TypeScript.
- A serialização dos dados deve utilizar a biblioteca zod.
- Utilizar um banco de dados PostgreSQL para a elaboração da API.
- Utilizar TypeORM no lugar de PG e PG-Format.
- Os nomes das tabelas, entidades, colunas e demais especificações devem ser seguidas à risca conforme o diagrama fornecido.
- Ter atenção aos nomes das chaves nos objetos de entrada e saída de cada requisição.
- A entrega deve conter testes automatizados, por isso, recomenda-se começar com os testes desde o início do projeto.
- A execução dos testes a cada criação de rota ajuda no debug e no andamento do projeto.
- Evitar alterações nos testes fornecidos, apenas acrescentar os arquivos que forem necessários.

## Rubrica

- Será avaliado o cumprimento de todos os requisitos do serviço.
- Será verificada a correta criação das entidades e seus relacionamentos.
- A correta implementação dos endpoints será avaliada, garantindo a funcionalidade correta de cada rota.
- A utilização adequada do TypeORM e a integração com o banco de dados serão verificadas.
- A implementação dos testes automatizados será avaliada, garantindo a cobertura dos cenários necessários.
- Qualquer tentativa de plágio ou interferência na entrega de outro aluno resultará em penalidades.
- A entrega é individual e os testes não devem ser alterados, apenas acrescentados arquivos necessários.
