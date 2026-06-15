# Online Store Simulator

> Simple simulator for an online store. Runs a Java backend and a Svelte frontend using Docker Compose.

## Overview

- Backend: Java Spring application (in `java-backend`).
- Frontend: Svelte app (in `svelte-frontend`).

This repository includes a `docker-compose.yml` that builds and runs both services locally.

## Prerequisites

- Docker and Docker Compose installed.
- A MariaDB instance (local or MariaDB Cloud).

## Quick start

1. Create a `.env` file next to `docker-compose.yml` (see example below).
2. Build and start the services:

```bash
docker-compose up --build
```

3. Stop the services:

```bash
docker-compose down
```

The backend is exposed on port `8080` by default and the frontend on port `3000` by default. You can override these with `BACKEND_PORT` and `FRONTEND_PORT` in your `.env`.

## Configuration — Database

The compose file maps database settings into the backend using these environment variables:

- `DB_HOST` — database host
- `DB_PORT` — database port
- `DB_NAME` — database name
- `DB_USER` — database username
- `DB_PASSWORD` — database password

Example `.env` (replace values):

```env
# Ports
BACKEND_PORT=8080
FRONTEND_PORT=3000

# Database (example)
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=demo
DB_USER=appuser
DB_PASSWORD=YourPasswordHere
```

The compose file constructs Spring variables from these (for example `SPRING_R2DBC_URL=r2dbc:mariadb://${DB_HOST}:${DB_PORT}/${DB_NAME}`). If your MariaDB provider gives you full connection strings, you can also set the Spring variables directly in `.env` to override:

```env
SPRING_R2DBC_URL=r2dbc:mariadb://HOST:PORT/DB
SPRING_LIQUIBASE_URL=jdbc:mariadb://HOST:PORT/DB
SPRING_R2DBC_USERNAME=USER
SPRING_R2DBC_PASSWORD=PASS
SPRING_LIQUIBASE_USER=USER
SPRING_LIQUIBASE_PASSWORD=PASS
```

### Using MariaDB Cloud

You can use a MariaDB Cloud serverless instance (there is a free tier for testing). Create an instance, then copy its host, port, database name, username and password into your `.env` as shown above.

If the provider requires TLS/extra parameters, you can set them after the value in the `DB_NAME` variable. For example:

```env
DB_NAME=demo?sslMode=trust
```

## Running a local MariaDB (optional)

If you just want to run a local MariaDB quickly for development, you can run:

```bash
docker run -d --name local-mariadb -e MARIADB_ROOT_PASSWORD=root -e MARIADB_DATABASE=demo -e MARIADB_USER=appuser -e MARIADB_PASSWORD=YourPasswordHere -p 3306:3306 mariadb:latest
```

Then set `DB_HOST=host.docker.internal` (on macOS) or `DB_HOST=127.0.0.1` and the rest of the DB variables accordingly.

## Notes

- The backend reads Liquibase (JDBC) and R2DBC settings for schema and runtime connections. The compose file sets both from the `DB_*` variables by default.
- If you prefer to run services individually, see each subfolder for service-specific instructions.
