# Student Management System API

## Demo Recording

A short walkthrough covering all four staff roles — Registrar, Librarian, Course Admin, and System Admin — recorded end-to-end against a live instance of the app.

[Demo Youtube](https://youtu.be/vD3ChtpMnwQ)

## What is this?

This repository contains the backend for a student management system. It is a Java 21 and Spring Boot API backed by MySQL. The application exposes versioned HTTP endpoints under `/api/v1`, uses Flyway for database migrations, and organizes code by business module.

## What does it support?

The API supports student registration and records, course management, enrollment, library book ownership, and staff accounts. Session based authentication and role based permissions separate the work of registrars, course administrators, librarians, system administrators, and students. Students can view their own profile, courses, and books through `/api/v1/me`.

## Start locally

You need JDK 21 and Docker with Compose. Run the following from the repository root:

```sh
cp .env.example .env
docker compose --profile dev up -d
./mvnw spring-boot:run
```

The first Maven run downloads Maven 3.9.16. MySQL is exposed on port `3306` and the API uses port `8080` by default. Flyway applies migrations in `src/main/resources/db/migration/` when the application starts. If MySQL is still starting, wait for `docker compose --profile dev ps` to show it as healthy, then rerun the API command.

In the default development profile, `GET /api/v1/auth/demo-accounts` lists seeded staff accounts for trying the API:

```sh
curl http://localhost:8080/api/v1/auth/demo-accounts
```

Use one of those accounts with `POST /api/v1/auth/login`; subsequent requests use the session cookie. The demo endpoint and seeder are disabled in the `prod` profile. Stop the local database with `docker compose --profile dev down`.

## Configuration and tests

`.env.example` contains local MySQL defaults; keep real credentials in an untracked `.env`. Spring reads `DB_HOST`, `DB_PORT`, `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD`, and `INITIAL_PASSWORD_KEY` from its process environment. If you change database values in `.env`, export the matching variables before starting Spring. Use a unique `INITIAL_PASSWORD_KEY` outside local development.

```sh
./mvnw test      # unit and integration tests; Docker is needed for Testcontainers
./mvnw verify    # tests plus a JaCoCo report in target/site/jacoco/
./mvnw package   # executable JAR in target/
```

Production code lives in `src/main/java/org/phuchoang/management/`; matching tests live in `src/test/java/`. The `student`, `course`, `enrollment`, `book`, and `identity` modules each separate domain, application, persistence, and web concerns.
