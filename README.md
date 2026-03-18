# Inkeep Agents — Railway One-Click Deploy

Deploy the full [Inkeep](https://inkeep.com) Agent Framework to [Railway](https://railway.com) with a single click.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new/template/QoQfOF)

---

## What Gets Deployed

This template provisions **8 services** wired together automatically:

| Service | Image | Port | Description |
|---|---|---|---|
| `agents-manage-ui` | `inkeep/agents-manage-ui:latest` | 3000 | Visual builder UI |
| `agents-api` | `inkeep/agents-api:latest` | 3002 | Core API (manage + run) |
| `agents-migrate` | `inkeep/agents-migrate:latest` | — | One-time DB migration job |
| `doltgres-db` | `dolthub/doltgresql:latest` | 5432 | Git-versioned Postgres (manage DB) |
| `postgres-db` | `postgres:18` | 5432 | Standard Postgres (run DB) |
| `spicedb` | `authzed/spicedb:latest` | 50051 | Authorization engine |
| `spicedb-migrate` | `authzed/spicedb:latest` | — | One-time SpiceDB migration job |
| `spicedb-postgres` | `postgres:16-alpine` | 5432 | Postgres for SpiceDB |

---

## Required Variables (set before deploy)

| Variable | Description |
|---|---|
| `ANTHROPIC_API_KEY` | Your Anthropic API key (at least one AI key required) |
| `OPENAI_API_KEY` | Your OpenAI API key (optional if using Anthropic) |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Your Google Gemini API key (optional) |
| `INKEEP_AGENTS_MANAGE_UI_USERNAME` | Admin email for initial login (e.g. `admin@example.com`) |
| `INKEEP_AGENTS_MANAGE_UI_PASSWORD` | Admin password for initial login |

All other secrets (JWT keys, SpiceDB preshared key, etc.) are **auto-generated** by Railway's `${{secret()}}` template functions.

---

## After Deploy

1. Open the `agents-manage-ui` service URL (Railway will assign a public domain)
2. Log in with your `INKEEP_AGENTS_MANAGE_UI_USERNAME` and `INKEEP_AGENTS_MANAGE_UI_PASSWORD`
3. Start building agents in the Visual Builder or push agents via the CLI:

```bash
npx @inkeep/create-agents my-agents
cd my-agents
# Configure inkeep-cloud.config.ts with your Railway URLs
inkeep push --config src/inkeep-cloud.config.ts
```

---

## Architecture

```
                    ┌─────────────────────────────────────┐
                    │           Railway Project            │
                    │                                      │
  Browser ──────────►  agents-manage-ui (:3000)           │
                    │         │                            │
                    │         ▼                            │
                    │  agents-api (:3002)                  │
                    │    │         │         │             │
                    │    ▼         ▼         ▼             │
                    │ doltgres  postgres  spicedb           │
                    │  (manage)  (run)  (authz)            │
                    └─────────────────────────────────────┘
```

---

## Docs

- [Inkeep Docs](https://docs.inkeep.com)
- [Talk to Your Agents](https://docs.inkeep.com/talk-to-your-agents/overview)
- [Railway Templates](https://docs.railway.com/templates/create)

---

## License

Inkeep is [fair-code licensed](https://github.com/inkeep/agents/blob/main/LICENSE.md). See the [Inkeep license](https://github.com/inkeep/agents/blob/main/LICENSE.md) for details.
