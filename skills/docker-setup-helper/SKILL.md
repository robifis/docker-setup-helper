---
name: docker-setup-helper
description: Help beginners safely set up Docker apps from GitHub repos with staged checks, env setup, ports, health checks, and approval gates.
---

# Docker Setup Helper

Use this when someone gives you a GitHub repository and wants help setting it up with Docker. The user may be a beginner. Keep them involved at every stage, explain decisions in simple terms, and do not perform risky actions without approval.

This is the Claude version of the skill. It follows Claude skill requirements: the folder name should match `docker-setup-helper`, this file should be named `SKILL.md`, and the frontmatter description is intentionally short.

## Ground Rules

- Inspect first, explain what you found, then ask before making decisions.
- Use beginner-friendly wording over Docker jargon.
- Preserve existing files and values. Never overwrite `.env`.
- Never run `docker compose up`, migrations, seed scripts, delete commands, or public exposure changes without approval.
- Never delete containers, images, volumes, or networks unless the user explicitly asks.
- Never invent external credentials such as API keys, SMTP passwords, OAuth secrets, or cloud tokens.
- Generate local-only secrets only after explaining what they are for.
- Treat the repo docs as the source of truth, but call out unsafe or confusing instructions.
- **Validation first**: Always verify the final `docker-compose.yml` configuration using `docker compose config` before suggesting the user run it.
- **Safety Checklist**: Before execution, explicitly confirm that the config is valid, no critical files are being overwritten, and the user has approved the specific port mapping.

## Stage 1: Repo Review

Inspect the repository before asking questions the repo can answer.

Look for:

- `README.md`, `docs/`, deploy guides, install guides.
- `compose.yaml`, `compose.yml`, `docker-compose.yml`, override files.
- `Dockerfile`, `.dockerignore`.
- `.env.example`, `.env.sample`, `.env.template`.
- Package manifests such as `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `Gemfile`, `composer.json`.
- Config mentioning databases, Redis, SMTP, object storage, OAuth, domains, workers, queues, or reverse proxies.

Explain:

- what the app appears to be,
- whether Docker support already exists,
- which file looks like the main setup file,
- whether the docs are clear enough to proceed.

Ask simply:

```text
Where do you want to deploy this: your local computer, a home server, or a VPS?
```

## Stage 2: System Check

Check:

- Docker is installed.
- Docker daemon is running.
- Docker Compose plugin is available.
- Operating system and CPU architecture.
- Available memory and disk space when discoverable.
- Required ports are free.

Explain missing items plainly:

```text
Docker is installed, but the Docker service is not running. That means containers cannot start yet.
```

Ask before suggesting installation or upgrade steps.

## Stage 3: App Requirements

Translate services into plain English:

- `web`: the main app you open in the browser.
- `postgres`, `mysql`, `mariadb`: the database where app data is saved.
- `redis` or `valkey`: a fast cache or queue helper.
- `worker`: background jobs such as email, imports, or scheduled tasks.
- `minio`: local S3-style file storage.
- `caddy`, `traefik`, `nginx`: reverse proxy or web gateway.

Ask simple decisions only when needed:

```text
Do you want the database included in Docker, or do you already have a database you want to use?
```

```text
Do you want this app available only on this machine, or also from other devices on your network?
```

For beginners, recommend the safest path: local-only access, database included in Docker, generated local secrets, and named Docker volumes for persistent data.

## Stage 4: Environment Setup

Use `.env.example`, docs, and compose variable references to build a proposed `.env`.

Rules:

- If `.env` exists, read it and preserve all values.
- If `.env` is missing, propose creating it from the example/template.
- Generate local-only secrets for values such as `APP_SECRET`, `JWT_SECRET`, `SECRET_KEY`, `SESSION_SECRET`, and database passwords.
- Mark external credentials as `USER_REQUIRED`, not fake values.
- Do not print full real secrets from an existing `.env`; summarize whether they are present.

Ask before filling decisions:

```text
The app needs an email provider. Do you want to skip email for now if the app allows it, or do you have SMTP details ready?
```

```text
The app needs a public URL for login callbacks. Do you have a domain name, or should we keep this local for now?
```

## Stage 5: Port Planning

Find ports from compose files, Dockerfiles, docs, and env examples.

Check whether host ports are in use. If a port is busy, suggest a nearby alternative.

Explain the effect:

```text
Port 3000 is already being used, so this app cannot use http://localhost:3000. I suggest changing it to 3001, which would make the app open at http://localhost:3001.
```

Ask before changing:

```text
Do you want me to change the app port from 3000 to 3001 in the proposed compose file?
```

For network exposure:

- Prefer `127.0.0.1:PORT:PORT` for local-only access.
- Warn before `0.0.0.0` or bare `PORT:PORT` on servers.
- Ask before exposing databases or admin tools to a network.

## Stage 6: Health Checks

Inspect existing `healthcheck:` entries. Add proposed health checks where practical and explain what they do.

Recommended checks:

- HTTP apps: use `curl`, `wget`, or a built-in app health command against `/health`, `/healthz`, `/api/health`, or the root page.
- Postgres: `pg_isready`.
- MySQL/MariaDB: `mysqladmin ping`.
- Redis/Valkey: `redis-cli ping`.
- Meilisearch: HTTP `GET /health`.
- Typesense: HTTP `GET /health`.
- Elasticsearch/OpenSearch: HTTP cluster health endpoint.

Do not invent fragile checks:

- If the image does not include `curl`, `wget`, or the relevant CLI, either use a built-in command from the image docs or explain that a health check is not safe to add without changing the image.
- Do not mark a service healthy by checking only that the process exists unless no better practical check exists.
- Do not add checks that require real external services or credentials.

Ask before adding:

```text
This app does not have health checks. I can propose checks so Docker can tell when the database and web app are ready. Do you want those added to the compose patch?
```

## Stage 7: Proposed Changes

Before editing files, show:

- files that would be created,
- files that would be changed,
- exact patch or clear before/after snippets,
- generated values that are safe local secrets,
- values the user must provide.

Ask:

```text
Do you want me to apply these file changes now?
```

## Stage 8: Startup Approval

Before running startup commands, show the exact commands and what each does.

**Validation Step**:
Before the final approval, run `docker compose config` (or equivalent read-only check) to ensure the proposed compose file is syntactically correct. Report the result to the user.

Typical command order:

```bash
docker compose pull
docker compose build
docker compose up -d
docker compose ps
docker compose logs --tail=100
```

Ask:

```text
Do you want me to start the containers now?
```

If migrations or seed commands are documented, ask separately:

```text
The app docs say to run a database migration. This changes the database schema. Do you want to run it now?
```

## Stage 9: Post-Deployment Summary (Starter Guide)

Once the app is running and healthy, create a `summary.md` file in the project root. This serves as a beginner's "Cheat Sheet".

Include:
- **Access URLs**: The exact URL(s) to open the app (e.g., `http://localhost:8080`).
- **Login Info**: Default admin usernames/passwords discovered during repo review.
- **Quick Commands**: How to stop, start, and view logs (e.g., `docker compose stop`, `docker compose logs -f`).
- **Common Pitfalls**: Specific warnings based on the app's requirements (e.g., "If the app is slow, increase Docker memory to 4GB").
- **Next Steps**: Any manual configuration needed after startup.

Inform the user:
```text
I've created a summary.md file for you. It contains your login details and the URLs to access your new app!
```

## Final Output Template

End every setup pass with a structured runbook:

```markdown
# Deployment Runbook: [App Name]

## 📋 Prerequisites
- [ ] Docker installed
- [ ] Docker Compose plugin available
- [ ] Memory/Disk space sufficient

## 🔍 What I Found In The Repo
- Main setup file: `...`
- Dependencies: `...`

## ⚙️ Environment & Ports
- Env file created: Yes/No
- Host Ports: `...`
- Publicly exposed: Yes/No

## 🛡️ Safety Checklist
- [ ] `docker compose config` validated
- [ ] `.env` preserved
- [ ] Port conflicts resolved
- [ ] User approved changes

## 🚀 Execution Plan
1. `docker compose pull`
2. `docker compose up -d`
3. [Migration/Seed steps]

## 🏁 Verification
- Expected URL: `...`
- Health check: `...`

## 🛠️ Troubleshooting
[Common pitfalls based on the repo]
```

## Troubleshooting Prompts

Use short, actionable explanations:

- Docker daemon not running: tell the user Docker is installed but not awake.
- Port conflict: identify the process if possible and offer a different port.
- Missing `.env`: propose creating one from the example file.
- Database password mismatch: explain that app and database passwords must match.
- Container exits immediately: inspect logs before guessing.
- ARM vs x86 image issue: explain that the image may not support the user's CPU.
- Low memory: explain which service is likely too heavy and suggest disabling optional services if docs allow.
- Health check failing: inspect logs and test the health command inside the container before changing it.
