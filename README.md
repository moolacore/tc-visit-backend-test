# DDIN backend services

This repository contains the DDIN API gateway and its Auth, Agents, Visits, and Reports services, plus their shared code and Prisma schema. Each service has its own `Dockerfile`; build from this repository root so the service can include the shared code and schema it needs.

Copy `.env.example` to `.env` and provide the deployment secrets and API credentials. Run `docker compose up --build -d` from this repository root to start the backend services; only the API gateway is exposed on port 3000. Agent creation and bulk agent registration remain disabled unless `AGENT_REGISTRATION_ENABLED=true` is set after the agent API is available.

The database schema and migrations are in `prisma/`. The Compose setup applies pending migrations before starting services after a schema update.
