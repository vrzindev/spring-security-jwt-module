# Módulo de Autenticação JWT com Spring Security

Idioma: [English](README.md) | **Português (Brasil)**

API REST de autenticação de usuários construída com **Spring Boot 3**, **Spring Security** e **JWT**, usando **PostgreSQL** e migrações com **Flyway**.

## Visão Geral

Este módulo oferece:

- Cadastro de usuário (`POST /auth/register`)
- Login de usuário (`POST /auth/login`)
- Geração de token JWT para acesso a endpoints protegidos

Os endpoints de autenticação são públicos. Todos os demais endpoints exigem um bearer token válido.

## Stack Tecnológica

- Java 21
- Spring Boot 3.2.4
- Spring Security
- Spring Data JPA
- PostgreSQL
- Flyway
- Auth0 Java JWT
- Maven

## Pré-requisitos

Certifique-se de ter:

- JDK 21
- Maven 3.9+ (opcional se usar Maven Wrapper)
- PostgreSQL em execução local

## Configuração

Propriedades da aplicação (`src/main/resources/application.properties`):

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/letroca
spring.datasource.username=postgres
spring.datasource.password=root

api.security.token.secret=${JWT_SECRET:my-secret-key}
```

Ajuste as credenciais do banco conforme seu ambiente local.

### Segredo JWT

Defina `JWT_SECRET` (recomendado):

**Windows (PowerShell)**

```powershell
$env:JWT_SECRET="seu-segredo-forte"
```

**Linux/macOS (bash)**

```bash
export JWT_SECRET="seu-segredo-forte"
```

Se não for definida, a aplicação usa `my-secret-key` como fallback.

## Executar a Aplicação

Na raiz do projeto:

```bash
./mvnw spring-boot:run
```

No Windows (cmd/PowerShell):

```bash
mvnw.cmd spring-boot:run
```

URL da aplicação: `http://localhost:8080`

## Migrações de Banco

Os scripts de migração estão em:

`src/main/resources/db/migration`

O Flyway executa as migrações automaticamente na inicialização.

## Endpoints de Autenticação

Base URL: `http://localhost:8080`

### Cadastrar Usuário

`POST /auth/register`

Corpo da requisição:

```json
{
    "name": "João da Silva",
    "email": "joao@example.com",
    "password": "123456",
    "role": "USER"
}
```

Exemplo com cURL:

```bash
curl -X POST http://localhost:8080/auth/register \
    -H "Content-Type: application/json" \
    -d '{"name":"João da Silva","email":"joao@example.com","password":"123456","role":"USER"}'
```

### Login

`POST /auth/login`

Corpo da requisição:

```json
{
    "email": "joao@example.com",
    "password": "123456"
}
```

Exemplo com cURL:

```bash
curl -X POST http://localhost:8080/auth/login \
    -H "Content-Type: application/json" \
    -d '{"email":"joao@example.com","password":"123456"}'
```

Exemplo de resposta:

```json
{
    "token": "<jwt-token>"
}
```

## Acessar Endpoints Protegidos

Envie o JWT no header `Authorization`:

```http
Authorization: Bearer <jwt-token>
```

Exemplo:

```bash
curl http://localhost:8080/your-protected-endpoint \
    -H "Authorization: Bearer <jwt-token>"
```

## Rodar Testes

```bash
./mvnw test
```

## Estrutura do Projeto (resumo)

- `controllers/AuthenticationController.java`: endpoints de cadastro e login
- `infra/security/`: filtro JWT, configuração de segurança e serviço de token
- `repositories/UserRepository.java`: acesso aos dados de usuários
- `entities/users/`: entidade, enum de perfil e DTOs

## Licença

Adicione a licença de sua preferência (por exemplo, MIT) se o projeto for distribuído publicamente.
