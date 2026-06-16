# Authentication Backend

This repository contains a robust and secure authentication backend service built with Java and Spring Boot. It provides a complete solution for user management, authentication, and authorization, featuring both local (email/password) and social (OAuth2) login mechanisms.

## Features

- **JWT Authentication:** Secure, stateless authentication using JSON Web Tokens (JWT) for both access and refresh tokens.
- **Role-Based Access Control (RBAC):** Pre-configured `ADMIN` and `GUEST` roles to protect API endpoints.
- **Local Authentication:** Standard user registration and login with email and password.
- **Social Login (OAuth2):** Integrated with Google for seamless social sign-in. Easily extendable for other providers like GitHub.
- **Secure Token Management:**
    - Refresh token rotation for enhanced security.
    - Refresh tokens are stored in the database and can be revoked.
    - Refresh tokens are transmitted via secure, HTTP-only cookies.
- **User Management:** Full CRUD (Create, Read, Update, Delete) operations for user accounts, accessible by admin users.
- **API Documentation:** Integrated Swagger (OpenAPI 3) for clear and interactive API documentation.
- **Custom Exception Handling:** Global exception handlers for consistent and informative error responses.
- **Database Initialization:** Automatically creates default `ADMIN` and `GUEST` roles on startup.

## Tech Stack

- **Backend:** Java 21, Spring Boot 3
- **Security:** Spring Security 6 (JWT, OAuth2, Password Encryption)
- **Database:** Spring Data JPA, MariaDB/MySQL
- **API:** Spring Web
- **Tooling:** Maven, Lombok
- **API Docs:** SpringDoc OpenAPI
- **JWT Library:** `jjwt`

## Getting Started

### Prerequisites

- JDK 21 or later
- Maven 3.x
- A running instance of MariaDB or MySQL.

### Configuration

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/suj018300/auth-backend.git
    cd auth-backend
    ```

2.  **Configure the database:**
    Open `src/main/resources/application-dev.yml` and update the `spring.datasource` properties with your database URL, username, and password. A database named `auth_app` is expected.

    ```yaml
    spring:
      datasource:
        url: jdbc:mariadb://localhost:3306/auth_app
        username: your_db_user
        password: your_db_password
    ```

3.  **Set up Environment Variables:**
    The application uses environment variables for sensitive credentials. For local development, you can set these in your IDE's run configuration.

    - **Google OAuth2:**
      ```
      GOOGLE_CLIENT_ID=<your-google-client-id>
      GOOGLE_CLIENT_SECRET=<your-google-client-secret>
      ```
    - **JWT Secret:**
      You can use the default development secret in `application-dev.yml` or set a stronger one.
      ```
      JWT_SECRET=<your-super-strong-jwt-secret-key>
      ```

### Running the Application

You can run the application using the Maven wrapper included in the project.

```sh
./mvnw spring-boot:run
```

The application will start on port `8083` (as configured in `application-dev.yml`).

## API Endpoints

Once the application is running, you can access the interactive API documentation at:
**Swagger UI:** `http://localhost:8083/swagger-ui.html`

### Authentication (`/api/v1/auth`)

-   `POST /register`: Creates a new user with the `GUEST` role.
-   `POST /login`: Authenticates a user and returns an access token in the response body and a refresh token in a secure cookie.
-   `POST /refresh`: Generates a new access token using a valid refresh token.
-   `POST /logout`: Revokes the user's refresh token and clears the security context.

### Social Login

-   Navigate to `/oauth2/authorization/google` to initiate the Google login flow.

### User Management (`/api/v1/users`)

These endpoints are protected and require an `ADMIN` role.

-   `GET /`: Retrieves a list of all users.
-   `POST /`: Creates a new user.
-   `GET /{userId}`: Retrieves a user by their ID.
-   `GET /email/{email}`: Retrieves a user by their email.
-   `POST /{userId}`: Updates an existing user's details.
-   `DELETE /{userId}`: Deletes a user.

## Project Structure

```
.
└── src/main/java/com/example/auth_app_backend/
    ├── config/          # Spring Security, OpenAPI, App Constants
    ├── controllers/     # REST API endpoints
    ├── dtos/            # Data Transfer Objects
    ├── entities/        # JPA entity classes (User, Role, RefreshToken)
    ├── exceptions/      # Global exception handlers
    ├── helpers/         # Utility classes
    ├── repositories/    # Spring Data JPA repositories
    ├── security/        # JWT services, UserDetailsService, OAuth2 handler
    └── services/        # Business logic and service interfaces/implementations
