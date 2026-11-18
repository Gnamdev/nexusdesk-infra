# Infrastructure

Local infrastructure for NexusDesk: **Keycloak** (identity) and the
**PostgreSQL** databases each service uses.

## Files

| File | Purpose |
|---|---|
| `keycloak/docker-compose.yml` | Keycloak 26.8.2 container |
| `keycloak/.env.example` | Template for environment variables |

## Keycloak

```bash
cp keycloak/.env.example keycloak/.env   # edit values as needed
docker compose -f keycloak/docker-compose.yml up -d
```

Keycloak starts at `http://localhost:8080` with the `nexusdesk` realm
(issuer URI `http://localhost:8080/realms/nexusdesk`).

## Databases

Each service owns its own database. JPA `ddl-auto: update` creates tables on
startup, so no migrations are needed during development:

| Service | Database |
|---|---|
| workspace-service | `workspace_db` |
| ticket-service | `tickets_db` |
| keycloak | `keycloak_db` |

## Environment variables

Copy `.env.example` to `.env` and adjust. **Never commit a real `.env`** — it is
already in `.gitignore`.

## Notes

- `host.docker.internal` is used so the Keycloak container can reach the host's
  PostgreSQL. Adjust `KC_DB_URL` for your setup.
- The `KEYCLOAK_ADMIN_PASSWORD` and `KC_DB_PASSWORD` values are local dev
  defaults; override them via `.env` for any shared environment.