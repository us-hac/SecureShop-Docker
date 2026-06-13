```markdown
# SecureShop — Dockerized Secure E-Commerce Application

A secure replica of OWASP Juice Shop, built to demonstrate identification and remediation of 8 common web application vulnerabilities. Fully containerized with Docker for one-command setup.

---

## Project Overview

SecureShop is a full-stack e-commerce application built as part of a **Cyber Security Internship project**. It demonstrates 8 OWASP vulnerabilities found in Juice Shop, each fixed with industry-standard security practices.

---

## Tech Stack

| Layer | Technology |
|------|------------|
| Frontend | React (Vite) + nginx |
| Backend | Java 21 + Spring Boot 3.5.14 |
| Database | MariaDB 11 |
| Security | BCrypt, JWT (HMAC-SHA256), Spring Security |
| Containerization | Docker + Docker Compose |

---

## Architecture

```

┌──────────────┐    ┌──────────────┐    ┌─────────────┐
│   Frontend   │───▶│   Backend    │───▶│  MariaDB    │
│  (nginx:80)  │    │ (Spring:8080)│    │  Database   │
│  Port 5173   │    │  Port 8080   │    │  Port 3307  │
└──────────────┘    └──────────────┘    └─────────────┘

````

---

## Security Fixes Implemented

| # | Vulnerability | Fix |
|---|---------------|-----|
| 1 | Cryptographic Weakness | BCrypt password hashing (cost factor 12) |
| 2 | SQL Injection | Spring Data JPA parameterized queries |
| 3 | Weak JWT Token Validation | HMAC-SHA256 signed tokens with expiry |
| 4 | Broken Access Control / IDOR | Principal-based ownership verification |
| 5 | Negative Quantity / Price Manipulation | Server-side validation, DB-trusted pricing |
| 6 | Reflected XSS | React auto-escaping + backend sanitization |
| 7 | DOM-Based XSS | DOMPurify sanitization |
| 8 | Information Disclosure | Global exception handler |

---

## Prerequisites

- Docker
- Docker Compose

---

## Quick Start

Clone this repository:

```bash
git clone https://github.com/us-hac/SecureShop-Docker.git
cd SecureShop-Docker
````

Build and start all containers:

```bash
docker-compose up --build
```

This starts 3 containers:

* `secureshop-db` — MariaDB database (port 3307)
* `secureshop-backend` — Spring Boot API (port 8080)
* `secureshop-frontend` — React app served via nginx (port 5173)

---

## Access the Application

Open your browser:

```
http://localhost:5173
```

---

## First-Time Setup — Adding Sample Products

On first run, the database is empty. Add sample products by connecting to the database container:

```bash
docker exec -it secureshop-db mariadb -uroot -prootpassword secureshop
```

Then run:

```sql
INSERT INTO products (name, description, price, imageurl) VALUES
('Apple Juice', 'Fresh apple juice 1L', 99.99, ''),
('Orange Juice', 'Fresh orange juice 1L', 89.99, ''),
('Mango Juice', 'Tropical mango juice 1L', 109.99, ''),
('Grape Juice', 'Natural grape juice 1L', 119.99, ''),
('Lemon Juice', 'Fresh lemon juice 500ml', 79.99, '');
exit;
```

---

## Creating an Admin User

1. Register a normal account through the UI
2. Connect to the database:

```bash
docker exec -it secureshop-db mariadb -uroot -prootpassword secureshop
```

3. Promote the user to admin:

```sql
UPDATE users SET role = 'ADMIN' WHERE email = 'your-email@example.com';
exit;
```

---

## Common Commands

| Command                             | Description                                 |
| ----------------------------------- | ------------------------------------------- |
| `docker-compose up -d`              | Start all containers in background          |
| `docker-compose down`               | Stop all containers (data persists)         |
| `docker-compose down -v`            | Stop containers AND delete database data    |
| `docker-compose logs -f backend`    | View live backend logs                      |
| `docker-compose up --build backend` | Rebuild only the backend after code changes |
| `docker ps`                         | List running containers                     |

---

## Project Structure

```
SecureShop/
├── backend/
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/
├── frontend/
│   ├── Dockerfile
│   └── src/
├── db-init/
│   └── init.sql
└── docker-compose.yml
```

---

## Data Persistence

Database data is stored in a Docker volume (`db_data`) and persists across container restarts.

Use `docker-compose down -v` only if you want to completely reset the database.

---

## License

This project was developed as part of a **Cyber Security Internship** for educational and demonstration purposes.

```

---

If you want, I can also:
- :contentReference[oaicite:0]{index=0}
- :contentReference[oaicite:1]{index=1}
- or :contentReference[oaicite:2]{index=2}

Just tell me.
```
