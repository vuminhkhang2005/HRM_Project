# HRM Project

HRM Project is a human resource management system built with Spring Boot. The application provides a static HTML interface, REST APIs, and business modules for employee management, departments, attendance, leave requests, payroll, performance reviews, notifications, and an internal AI HR assistant.

The main application source code is located in `HumanResourceManagementWebsite`.

## Main Features

- User authentication with JWT access tokens and refresh tokens.
- Role-based access control for `ADMIN`, `HR`, `MANAGER`, and `EMPLOYEE`.
- Employee, department, manager assignment, and avatar management.
- Check-in/check-out attendance tracking, attendance history, automatic daily closing, and leave synchronization.
- Leave request creation, approval, and rejection workflows.
- Monthly payroll generation, deductions, payment status tracking, and Excel export.
- Employee performance reviews by year/quarter, including submit and approval flows.
- HR dashboard with employee, department, attendance, and leave statistics.
- Internal notification system with optional SMTP email delivery.
- Gemini-powered HR AI assistant with guardrails, audit logging, policy knowledge, and rate limiting.
- Excel exports for employees, attendance, and payroll using Apache POI.
- Static web pages for login, dashboard, employees, departments, attendance, leave, payroll, performance, profile, admin users, and the AI assistant.

## Tech Stack

- Java 17
- Spring Boot 4.0.3
- Spring Web MVC
- Spring Security
- Spring Data JPA / Hibernate
- MySQL 8
- H2 for tests
- Maven Wrapper
- JJWT
- Lombok
- Apache POI
- Spring Mail
- Gemini API
- Docker

## Project Structure

```text
.
|-- deploy.docx
|-- README.md
`-- HumanResourceManagementWebsite
    |-- pom.xml
    |-- mvnw / mvnw.cmd
    |-- Dockerfile
    |-- hrm_db.sql
    |-- seed_data.sql
    |-- src
        |-- main
        |   |-- java/org/example/hrmsystem
        |   |   |-- ai
        |   |   |-- config
        |   |   |-- controller
        |   |   |-- dto
        |   |   |-- exception
        |   |   |-- model
        |   |   |-- repository
        |   |   |-- security
        |   |   `-- service
        |   `-- resources
        |       |-- application.properties
        |       |-- ai/policy
        |       `-- static
        `-- test
```

## Requirements

- JDK 17 or newer to build and run the application with Maven.
- MySQL 8.0 or newer.
- Maven is optional because the project includes Maven Wrapper.
- Docker if you want to build and run a container image.
- Gemini API key and SMTP account if AI/email features are enabled.

## Configuration

Default configuration file:

```text
HumanResourceManagementWebsite/src/main/resources/application.properties
```

For local development or deployment, override sensitive values with environment variables:

| Environment variable | Description |
| --- | --- |
| `PORT` | Server port. Default: `8080`. |
| `JWT_SECRET` | JWT signing secret. Use a long, environment-specific value. |
| `JWT_EXPIRATION_MS` | Access token lifetime. |
| `REFRESH_TOKEN_MAX_AGE_SECONDS` | Refresh token lifetime. |
| `REFRESH_TOKEN_SECURE` | Enables the secure flag for refresh token cookies when using HTTPS. |
| `MAIL_SEND_ENABLED` | Enables or disables email delivery. |
| `MAIL_HOST`, `MAIL_PORT` | SMTP server configuration. |
| `MAIL_USERNAME`, `MAIL_PASSWORD` | SMTP credentials. |
| `MAIL_FROM` | Sender email address. |
| `GEMINI_API_KEY` | Gemini API key. |
| `GEMINI_MODEL_NAME` | Gemini model name. |
| `AI_GEMINI_ENABLED` | Enables or disables Gemini calls. |
| `AI_CHAT_MAX_MSG_PER_MIN` | AI chat rate limit per minute. |

Do not use default secrets in production. Configure secrets through environment variables, a local `.env` file, or your deployment platform.

## Database Setup

1. Create the schema and initial sample data:

```bash
mysql -u root -p < HumanResourceManagementWebsite/hrm_db.sql
```

2. Load additional seed data if needed:

```bash
mysql -u root -p hrm_db < HumanResourceManagementWebsite/seed_data.sql
```

3. Check the MySQL connection in `application.properties` or override it through environment variables/command-line arguments.

The project is configured for this local database by default:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/hrm_db
spring.datasource.username=sa
```

If your MySQL instance uses a different user or password, update the configuration before running the application.

## Run Locally

### Windows

```powershell
cd HumanResourceManagementWebsite
.\mvnw.cmd spring-boot:run
```

### macOS/Linux

```bash
cd HumanResourceManagementWebsite
./mvnw spring-boot:run
```

After the server starts, open:

- Home: `http://localhost:8080`
- Login: `http://localhost:8080/login`
- Dashboard: `http://localhost:8080/overview`
- AI Assistant: `http://localhost:8080/assistant.html`

## Sample Accounts

After importing `hrm_db.sql`, you can log in with the following accounts. The default password for all sample accounts is `123456`.

| Username | Role |
| --- | --- |
| `admin` | `ADMIN` |
| `hr1` | `HR` |
| `manager1` | `MANAGER` |
| `employee1` | `EMPLOYEE` |

After importing `seed_data.sql`, the project also includes more employee accounts by email, for example:

- `lan.nguyen@hrm.com`
- `minh.hoang@hrm.com`
- `long.phan@hrm.com`

## Main APIs

| Module | Base endpoint | Description |
| --- | --- | --- |
| Auth | `/api/auth` | Login, refresh token, and logout. |
| Dashboard | `/api/dashboard` | HRM summary statistics. |
| Employees | `/api/employees` | Employee CRUD, avatar upload, and Excel export. |
| Departments | `/api/departments` | Department CRUD and employees by department. |
| Attendance | `/api/attendance` | Check-in, check-out, history, and attendance export. |
| Leave | `/api/leave` | Leave request creation, listing, approval, and rejection. |
| Payroll | `/api/payroll` | Payroll listing, monthly generation, and Excel export. |
| Performance Reviews | `/api/performance-reviews` | Performance review management, submit, and approval. |
| Profile | `/api/profile` | Current user profile, profile update, and password change. |
| Notifications | `/api/notifications` | User notifications and mark-as-read actions. |
| Admin Users | `/api/admin/users` | User account management, lock/unlock, role, username, and password updates. |
| AI Assistant | `/api/ai/chat` | Chat endpoint for the HR AI assistant. |

Except for public endpoints such as login/status, most APIs require this header:

```http
Authorization: Bearer <access-token>
```

## Tests

Run tests with Maven Wrapper:

### Windows

```powershell
cd HumanResourceManagementWebsite
.\mvnw.cmd test
```

### macOS/Linux

```bash
cd HumanResourceManagementWebsite
./mvnw test
```

The test profile uses an H2 in-memory database and disables environment-dependent behavior such as email delivery and Gemini calls.

## Build and Docker

Build the JAR:

### Windows

```powershell
cd HumanResourceManagementWebsite
.\mvnw.cmd clean package
```

### macOS/Linux

```bash
cd HumanResourceManagementWebsite
./mvnw clean package
```

Build the Docker image from the `HumanResourceManagementWebsite` directory:

```bash
docker build -t hrm-system .
```

Run the container:

```bash
docker run --name hrm-system -p 8080:8080 --env-file .env hrm-system
```

When running with Docker, make sure the container can connect to MySQL and that all required values in `.env` are configured correctly.

## Deployment Notes

- `spring.jpa.hibernate.ddl-auto=none`, so the database must be initialized with SQL scripts before the application starts.
- Daily attendance closing is enabled by default with `hrm.attendance.daily-closing.enabled=true`.
- The default avatar upload directory is `uploads/avatars`.
- For production, configure environment-specific secrets and do not commit `.env` files.
- To enable email delivery, set `MAIL_SEND_ENABLED=true` and provide valid SMTP settings.
- To disable AI features, set `AI_GEMINI_ENABLED=false`.
