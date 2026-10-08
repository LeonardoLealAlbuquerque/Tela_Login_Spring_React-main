# Tela de Login com Spring Boot + React

Este repositório contém uma aplicação full stack para autenticação de usuários, com frontend em React e API REST em Java/Spring Boot. O projeto foi desenvolvido como exemplo de login, cadastro, autenticação por JWT e proteção de rotas no lado do cliente e servidor.

## Visão geral

A aplicação oferece:

- Cadastro de usuários
- Login com autenticação por username e senha
- Geração e validação de token JWT
- Rotas protegidas no frontend
- API segura com Spring Security
- Banco em memória H2 para desenvolvimento
- Integração entre frontend e backend via Axios e CORS

## Stack tecnológica

### Backend

- Java 17
- Spring Boot 2.5.4
- Spring Web
- Spring Security
- Spring Data JPA
- Hibernate
- JWT (Auth0)
- H2 Database
- PostgreSQL driver
- Lombok
- OpenAPI / Swagger UI

### Frontend

- React 18
- React Router DOM
- Axios
- Styled Components
- Create React App

## Estrutura do projeto

```text
Tela_Login_Spring_React-main/
├── backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── one/digitalinnovation/gof/
│   │   │   │       ├── controller/
│   │   │   │       ├── model/
│   │   │   │       ├── role/
│   │   │   │       ├── security/
│   │   │   │       └── service/
│   │   │   └── resources/
│   │   │       ├── application.properties
│   │   │       └── db/migrations/
│   │   └── test/
│   ├── pom.xml
│   ├── mvnw
│   ├── mvnw.cmd
│   └── README.md
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── contexts/
│   │   ├── pages/
│   │   ├── routes/
│   │   ├── styles/
│   │   ├── App.jsx
│   │   └── index.jsx
│   ├── package.json
│   ├── package-lock.json
│   └── README.md
├── .gitignore
└── README.md
```

## Componentes principais

### Backend

- `Application.java`: classe principal da API Spring Boot
- `AuthenticationController`: endpoints de autenticação
- `UserController`: consulta de usuários
- `User`: entidade de usuário implementando `UserDetails`
- `UserRepository`: camada de acesso ao banco
- `TokenService`: geração e validação do JWT
- `JWTFilter`: filtro para autenticação por token nas requisições
- `WebSecurityConfig`: configuração de segurança, CORS e permissões

### Frontend

- `AuthContext.jsx`: contexto global do usuário e funções de login/cadastro/logout
- `RoutesApp.jsx`: configuração das rotas públicas e privadas
- `Signin/index.jsx`: tela de login
- `Signup/index.jsx`: tela de cadastro
- `Home/index.jsx`: página principal após autenticação
- `Button` e `Input`: componentes reutilizáveis da interface

## Fluxo da aplicação

1. O usuário acessa a tela de login ou cadastro.
2. O frontend envia as credenciais para o backend na rota `/auth/login` ou `/auth/register`.
3. O backend valida as credenciais com Spring Security e, em caso de sucesso, retorna um token JWT.
4. O frontend salva o token no `localStorage` e mantém o usuário autenticado no contexto.
5. A partir daí, o usuário pode acessar a rota protegida `/home`.
6. O backend verifica o token em requisições protegidas com `JWTFilter`.

## Endpoints da API

A API está mapeada em `/auth`.

### POST `/auth/register`

Cria um novo usuário.

Exemplo de payload:

```json
{
  "username": "usuario",
  "password": "123456",
  "role": "USER"
}
```

### POST `/auth/login`

Autentica um usuário e retorna o token JWT.

Exemplo de payload:

```json
{
  "username": "usuario",
  "password": "123456"
}
```

Resposta esperada:

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

### GET `/auth/users`

Lista os usuários cadastrados.

## Modelos e entidades

### Usuário

A entidade `User` possui:

- `id`
- `username`
- `password`
- `role`

Os papéis definidos são:

- `USER`
- `ADMIN`

## Banco de dados

O projeto usa H2 em memória para desenvolvimento, configurado em `backend/src/main/resources/application.properties`.

Configuração principal:

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
api.security.token.secret=${JWT_SECRET:my-secret-key}
```

O console H2 pode ser acessado em:

- `http://localhost:8080/h2-console`

## Requisitos

Antes de executar o projeto, verifique se você possui:

- Java 17+
- Maven ou Maven Wrapper
- Node.js 18+
- npm

## Como executar

### 1. Backend

No diretório `backend`:

```bash
./mvnw spring-boot:run
```

Ou, se preferir:

```bash
mvn spring-boot:run
```

A API ficará disponível em:

- `http://localhost:8080`

Swagger/OpenAPI (quando disponível):

- `http://localhost:8080/swagger-ui.html`

### 2. Frontend

No diretório `frontend`:

```bash
npm install
npm start
```

A interface web ficará disponível em:

- `http://localhost:3000`

## Observações importantes

- O projeto foi pensado para estudo e demonstração, e não como solução pronta para produção.
- O valor padrão do segredo JWT está em `application.properties` como `my-secret-key` quando a variável `JWT_SECRET` não é informada.
- A configuração de CORS permite apenas `http://localhost:3000` para comunicação com o frontend.
- Há migrações de banco em `backend/src/main/resources/db/migrations`, incluindo uma tabela de produtos e outra de usuários.

## Possíveis melhorias futuras

- Implementar refresh token
- Melhorar validações de entrada e mensagens de erro
- Separar autenticação e autorização em módulos mais robustos
- Adicionar testes automatizados para backend e frontend
- Usar banco persistente em ambiente de produção (PostgreSQL, MySQL etc.)
- Criar telas de perfil, recuperação de senha e controle de acesso por roles

## Conclusão

Este projeto é um exemplo prático de autenticação full stack usando Spring Boot e React. Ele demonstra como combinar segurança, gerenciamento de usuários, JWT e interface web em uma aplicação simples e didática, ideal para aprendizado e extensão.
