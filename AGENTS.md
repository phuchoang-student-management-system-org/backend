# Repository Guidelines

## Project Structure & Module Organization

This repository currently contains a Java 21 Spring Boot API. Production code lives under `src/main/java/org/phuchoang/management/`, grouped by business module such as `student`, `book`, `course`, `enrollment`, and `identity`. Within each module, keep domain rules in `domain/`, use cases in `application/`, repository contracts in `port/`, persistence adapters in `internal/`, and HTTP code in `web/`. Tests mirror these packages under `src/test/java/`. Configuration and Flyway migrations live in `src/main/resources/`; name migrations `V<number>__description.sql`.

## Build, Test, and Development Commands

Run these from the repository root:

```sh
cp .env.example .env                         # create local database settings
docker compose --profile dev up -d           # start MySQL
./mvnw spring-boot:run                        # run the API locally
./mvnw test                                   # run unit and integration tests
./mvnw verify                                 # test and generate the JaCoCo report
./mvnw package                                # build the executable JAR
docker compose --profile dev down             # stop local MySQL
```

The current `Makefile` still references the former repository layout; use the direct commands above until it is updated.

## Coding Style & Naming Conventions

Follow existing Java style: two-space indentation, PascalCase types, and camelCase methods and fields. Name HTTP handlers `*Controller`, application services `*Service`, repository ports `*Repository`, and persistence records `*Row`; ArchUnit tests enforce these rules and module boundaries. Keep cross-module access through published interfaces rather than another module's `internal/` package. No standalone formatter or linter is configured; match nearby code and let compilation and architecture tests catch violations.

## Testing Guidelines

Use JUnit tests named `*Test` for focused domain, service, and controller behavior. Name database, HTTP, security, and module-spanning tests `*IntegrationTest`. Integration tests use MySQL through Testcontainers, so a working Docker daemon is required. Run `./mvnw verify` before opening a pull request; review the JaCoCo report in `target/site/jacoco/` when changing behavior. No minimum coverage threshold is configured.

## Commit & Pull Request Guidelines

Recent history favors short, scoped subjects such as `feat(*)`, `fix(*)`, `docs(benchmark)`, and `perf(*)`. Use an imperative summary and keep each commit focused. In pull requests, describe the behavior change, link a relevant issue, list validation commands, and include request and response examples for API changes. Call out schema changes and include the corresponding Flyway migration.

## Security & Configuration

Keep local credentials in `.env`, which is ignored by Git. Set a unique `INITIAL_PASSWORD_KEY` outside development; the value in `.env.example` is only a local default. Review `application-prod.properties` when changing authentication, demo accounts, or actuator exposure.

## Relevant project.

You can find the backend and document in [frontend](../frontend/) and [document](../docs/).

If you need them and can't find them. Clone it from [frontend](https://github.com/phuchoang-student-management-system-org/frontend.git) and [document](https://github.com/phuchoang-student-management-system-org/docs.git) to the directories above.
