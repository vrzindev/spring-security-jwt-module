# Spring Security JWT Authentication Module

Language: **English** | [Português (Brasil)](README.pt-BR.md)

REST API for user authentication built with **Spring Boot 3**, **Spring Security**, and **JWT**, backed by **PostgreSQL** and database migrations with **Flyway**.

## Overview

This module provides:

- User registration (`POST /auth/register`)
- User login (`POST /auth/login`)
- JWT token generation for protected endpoints

Authentication endpoints are public. All other endpoints require a valid bearer token.

## Tech Stack

- Java 21
- Spring Boot 3.2.4
- Spring Security
- Spring Data JPA
- PostgreSQL
- Flyway
- Auth0 Java JWT
- Maven

## Prerequisites

Make sure you have:

- JDK 21
- Maven 3.9+ (optional if using Maven Wrapper)
- PostgreSQL running locally

## Configuration

Application properties (`src/main/resources/application.properties`):

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/letroca
spring.datasource.username=postgres
spring.datasource.password=root

api.security.token.secret=${JWT_SECRET:my-secret-key}
```

Update database credentials to match your local environment.

### JWT Secret

Set `JWT_SECRET` (recommended):

**Windows (PowerShell)**

```powershell
$env:JWT_SECRET="your-strong-secret"
```

**Linux/macOS (bash)**

```bash
export JWT_SECRET="your-strong-secret"
```

If not set, the application uses `my-secret-key` as fallback.

## Run the Application

From the project root:

```bash
./mvnw spring-boot:run
```

On Windows (cmd/PowerShell):

```bash
mvnw.cmd spring-boot:run
```

Application URL: `http://localhost:8080`

## Database Migrations

Migration scripts are located at:

`src/main/resources/db/migration`

Flyway runs migrations automatically on startup.

## Authentication Endpoints

Base URL: `http://localhost:8080`

### Register User

`POST /auth/register`

Request body:

```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "123456",
    "role": "USER"
}
```

cURL example:

```bash
curl -X POST http://localhost:8080/auth/register \
    -H "Content-Type: application/json" \
    -d '{"name":"John Doe","email":"john@example.com","password":"123456","role":"USER"}'
```

### Login

`POST /auth/login`

Request body:

```json
{
    "email": "john@example.com",
    "password": "123456"
}
```

cURL example:

```bash
curl -X POST http://localhost:8080/auth/login \
    -H "Content-Type: application/json" \
    -d '{"email":"john@example.com","password":"123456"}'
```

Example response:

```json
{
    "token": "<jwt-token>"
}
```

## Access Protected Endpoints

Send the JWT in the `Authorization` header:

```http
Authorization: Bearer <jwt-token>
```

Example:

```bash
curl http://localhost:8080/your-protected-endpoint \
    -H "Authorization: Bearer <jwt-token>"
```

## Run Tests

```bash
./mvnw test
```

## Project Structure (summary)

- `controllers/AuthenticationController.java`: register and login endpoints
- `infra/security/`: JWT filter, security configuration, token service
- `repositories/UserRepository.java`: user data access
- `entities/users/`: entity, role enum, and DTOs

## License

Add your preferred license (for example, MIT) if this project is intended for public distribution.
