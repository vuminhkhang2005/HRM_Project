# HRM Project

HRM Project la he thong quan ly nhan su duoc xay dung bang Spring Boot. Ung dung cung cap giao dien HTML tinh, REST API va cac module nghiep vu cho quan ly nhan vien, phong ban, cham cong, nghi phep, bang luong, danh gia hieu suat, thong bao va tro ly AI noi bo.

Ma nguon chinh nam trong thu muc `HumanResourceManagementWebsite`.

## Tinh nang chinh

- Xac thuc nguoi dung bang JWT access token va refresh token.
- Quan ly tai khoan theo vai tro `ADMIN`, `HR`, `MANAGER`, `EMPLOYEE`.
- Quan ly nhan vien, phong ban, quan he quan ly va anh dai dien.
- Cham cong vao/ra, lich su cham cong, dong ngay cham cong tu dong va dong bo ngay nghi phep.
- Quan ly don nghi phep, phe duyet hoac tu choi don nghi.
- Tao bang luong theo thang, tinh khau tru, trang thai thanh toan va xuat Excel.
- Danh gia hieu suat nhan vien theo nam/quy, submit va approve review.
- Dashboard tong hop so lieu nhan su, phong ban, cham cong va nghi phep.
- He thong thong bao noi bo va tuy chon gui email SMTP.
- Tro ly AI HR su dung Gemini, co guardrail, audit log, policy knowledge va rate limit.
- Xuat Excel cho nhan vien, cham cong va bang luong bang Apache POI.
- Giao dien web tinh cho cac man hinh login, dashboard, employees, departments, attendance, leave, payroll, performance, profile, admin users va AI assistant.

## Cong nghe su dung

- Java 17
- Spring Boot 4.0.3
- Spring Web MVC
- Spring Security
- Spring Data JPA / Hibernate
- MySQL 8
- H2 cho test
- Maven Wrapper
- JJWT
- Lombok
- Apache POI
- Spring Mail
- Gemini API
- Docker

## Cau truc thu muc

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

## Yeu cau moi truong

- JDK 17 tro len de build va chay bang Maven.
- MySQL 8.0 tro len.
- Maven Wrapper da co san trong project, khong bat buoc cai Maven rieng.
- Docker neu muon build image container.
- Gemini API key va SMTP account neu bat tinh nang AI/email.

## Cau hinh

File cau hinh mac dinh: `HumanResourceManagementWebsite/src/main/resources/application.properties`.

Nen ghi de cac cau hinh nhay cam bang bien moi truong khi chay local hoac deploy:

| Bien moi truong | Y nghia |
| --- | --- |
| `PORT` | Port chay server, mac dinh `8080`. |
| `JWT_SECRET` | Secret ky JWT, nen dat chuoi dai va rieng cho moi moi truong. |
| `JWT_EXPIRATION_MS` | Thoi gian song access token. |
| `REFRESH_TOKEN_MAX_AGE_SECONDS` | Thoi gian song refresh token. |
| `REFRESH_TOKEN_SECURE` | Bat co secure cho refresh token cookie khi chay HTTPS. |
| `MAIL_SEND_ENABLED` | Bat/tat gui email. |
| `MAIL_HOST`, `MAIL_PORT` | Cau hinh SMTP server. |
| `MAIL_USERNAME`, `MAIL_PASSWORD` | Tai khoan SMTP. |
| `MAIL_FROM` | Dia chi email hien thi nguoi gui. |
| `GEMINI_API_KEY` | API key cho Gemini. |
| `GEMINI_MODEL_NAME` | Model Gemini su dung. |
| `AI_GEMINI_ENABLED` | Bat/tat tinh nang goi Gemini. |
| `AI_CHAT_MAX_MSG_PER_MIN` | Gioi han so tin nhan AI moi phut. |

Luu y: khong nen su dung secret mac dinh cho production. Hay tao file `.env` rieng hoac cau hinh bien moi truong tren nen tang deploy.

## Khoi tao database

1. Tao schema va du lieu mau ban dau:

```bash
mysql -u root -p < HumanResourceManagementWebsite/hrm_db.sql
```

2. Nap them seed data mo rong neu can:

```bash
mysql -u root -p hrm_db < HumanResourceManagementWebsite/seed_data.sql
```

3. Kiem tra ket noi MySQL trong `application.properties` hoac ghi de bang cau hinh moi truong/command line.

Project dang cau hinh database local mac dinh:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/hrm_db
spring.datasource.username=sa
```

Neu MySQL cua ban dung user/password khac, hay doi cau hinh truoc khi chay.

## Chay project local

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

Sau khi server khoi dong, truy cap:

- Trang chu: `http://localhost:8080`
- Dang nhap: `http://localhost:8080/login`
- Dashboard: `http://localhost:8080/overview`
- Tro ly AI: `http://localhost:8080/assistant.html`

## Tai khoan mau

Sau khi import `hrm_db.sql`, co the dang nhap bang cac tai khoan sau. Mat khau mac dinh cho tat ca tai khoan mau la `123456`.

| Username | Vai tro |
| --- | --- |
| `admin` | `ADMIN` |
| `hr1` | `HR` |
| `manager1` | `MANAGER` |
| `employee1` | `EMPLOYEE` |

Sau khi import them `seed_data.sql`, project co them nhieu tai khoan nhan vien theo email, vi du:

- `lan.nguyen@hrm.com`
- `minh.hoang@hrm.com`
- `long.phan@hrm.com`

## API chinh

| Module | Endpoint goc | Mo ta |
| --- | --- | --- |
| Auth | `/api/auth` | Dang nhap, refresh token, logout. |
| Dashboard | `/api/dashboard` | Thong ke tong quan HRM. |
| Employees | `/api/employees` | CRUD nhan vien, upload avatar, export Excel. |
| Departments | `/api/departments` | CRUD phong ban va danh sach nhan vien theo phong ban. |
| Attendance | `/api/attendance` | Check-in, check-out, lich su, export cham cong. |
| Leave | `/api/leave` | Tao don nghi, xem don, phe duyet, tu choi. |
| Payroll | `/api/payroll` | Xem bang luong, tao bang luong thang, export Excel. |
| Performance Reviews | `/api/performance-reviews` | Quan ly review hieu suat, submit va approve. |
| Profile | `/api/profile` | Xem/cap nhat ho so ca nhan va doi mat khau. |
| Notifications | `/api/notifications` | Lay thong bao va danh dau da doc. |
| Admin Users | `/api/admin/users` | Quan ly tai khoan nguoi dung, khoa/mo khoa, role, mat khau. |
| AI Assistant | `/api/ai/chat` | Chat voi tro ly AI HR. |

Ngoai cac API public nhu login/status, phan lon endpoint yeu cau header:

```http
Authorization: Bearer <access-token>
```

## Test

Chay test bang Maven Wrapper:

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

Test profile su dung H2 in-memory database va tat cac tac vu phu thuoc moi truong nhu gui email/Gemini.

## Build va Docker

Build file JAR:

```bash
cd HumanResourceManagementWebsite
./mvnw clean package
```

Build Docker image:

```bash
docker build -t hrm-system .
```

Chay container:

```bash
docker run --name hrm-system -p 8080:8080 --env-file .env hrm-system
```

Khi chay Docker, dam bao container co the ket noi den MySQL va cac bien moi truong trong `.env` da duoc cau hinh dung.

## Ghi chu trien khai

- `spring.jpa.hibernate.ddl-auto=none`, nen database can duoc khoi tao bang script SQL truoc khi chay app.
- Tinh nang dong ngay cham cong duoc bat mac dinh qua `hrm.attendance.daily-closing.enabled=true`.
- Thu muc upload avatar mac dinh la `uploads/avatars`.
- Neu chay production, can cau hinh secret rieng, tat secret mac dinh va khong commit file `.env`.
- Neu bat email, hay dat `MAIL_SEND_ENABLED=true` va cau hinh SMTP hop le.
- Neu tat AI, dat `AI_GEMINI_ENABLED=false`.
