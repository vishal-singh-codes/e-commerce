# E-Commerce API

Spring Boot 3 reference service for common e-commerce backend flows: authentication, role-based access, product catalog, cart management, address book, and order placement.

The project is intentionally focused on the backend API layer. It demonstrates Spring Security with JWT cookies, JPA/Hibernate persistence, request validation, pagination/sorting, global exception handling, and Postman collections for manual API verification.

## Capabilities

- User signup, signin, signout, and current-user lookup.
- JWT-backed authentication stored in an HTTP cookie.
- Role model for user, seller, and admin flows.
- Product and category APIs with pagination and sorting.
- Cart item add/update/delete flows.
- Address CRUD for authenticated users.
- Order creation with payment method capture.
- DTO-based request/response boundaries.
- Global exception handling for API errors.

## Stack

- Java 21
- Spring Boot 3.3
- Spring Web
- Spring Security
- Spring Data JPA
- Hibernate
- MySQL or H2
- JJWT
- ModelMapper
- Lombok
- Maven

## Main API Areas

| Area | Representative routes |
| --- | --- |
| Auth | `POST /api/auth/signup`, `POST /api/auth/signin`, `POST /api/auth/signout` |
| Products | `GET /api/public/products`, `POST /api/admin/categories/{categoryId}/product` |
| Categories | `GET /api/public/categories`, `POST /api/public/categories` |
| Cart | `POST /api/carts/products/{productId}/quantity/{quantity}`, `GET /api/carts/users/cart` |
| Address | `POST /api/addresses`, `GET /api/users/addresses` |
| Orders | `POST /api/order/users/payments/{paymentMethod}` |

## Run Locally

The default configuration uses an in-memory H2 database so the API can start without local credentials.

```bash
./mvnw spring-boot:run
```

Run tests:

```bash
./mvnw test
```

## Database Configuration

For MySQL, override the datasource with environment variables:

```bash
export ECOM_DATASOURCE_URL="jdbc:mysql://localhost:3306/local"
export ECOM_DATASOURCE_USERNAME="root"
export ECOM_DATASOURCE_PASSWORD="<local-password>"
export ECOM_DATASOURCE_DRIVER="com.mysql.cj.jdbc.Driver"
export ECOM_JPA_DIALECT="org.hibernate.dialect.MySQL8Dialect"
```

PowerShell equivalent:

```powershell
$env:ECOM_DATASOURCE_URL = "jdbc:mysql://localhost:3306/local"
$env:ECOM_DATASOURCE_USERNAME = "root"
$env:ECOM_DATASOURCE_PASSWORD = "<local-password>"
$env:ECOM_DATASOURCE_DRIVER = "com.mysql.cj.jdbc.Driver"
$env:ECOM_JPA_DIALECT = "org.hibernate.dialect.MySQL8Dialect"
```

JWT configuration is also environment-backed:

| Variable | Default |
| --- | --- |
| `ECOM_JWT_SECRET` | Local development Base64 secret |
| `ECOM_JWT_EXPIRATION_MS` | `3000000` |
| `ECOM_JWT_COOKIE_NAME` | `springBootEcom` |
| `ECOM_SEED_USER_PASSWORD` | `change-me-user` |
| `ECOM_SEED_SELLER_PASSWORD` | `change-me-seller` |
| `ECOM_SEED_ADMIN_PASSWORD` | `change-me-admin` |

Use a strong Base64-encoded JWT secret and non-default seed passwords outside local development.

## Postman

Collections live under `src/main/resources/postman/`:

- Auth
- Product
- Category
- Cart
- Address
- Order

Import the relevant collection into Postman, set `baseUrl` to `http://localhost:8080`, then sign in to capture a JWT for authenticated endpoints.

## Project Layout

```text
src/main/java/com/ecommerce/project
|-- controller/     REST controllers
|-- model/          JPA entities and roles
|-- payload/        DTOs and API response payloads
|-- repositories/   Spring Data repositories
|-- security/       JWT filter, entrypoint, user details, role setup
|-- service/        Business operations for catalog, cart, address, and order flows
|-- exceptions/     Global exception handling
```
