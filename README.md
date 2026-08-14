# TRIC system of record: functional Next.js reconstruction

<img width="1909" height="945" alt="image" src="https://github.com/user-attachments/assets/b44d3cac-608b-4f45-bc82-24f3df947223" />

This repository is an independent functional reconstruction of a public security operations product. It uses original code, original CSS and seeded demo data. It does **not** contain third-party source code, private prompts, private procedure text, credentials, customer data or reverse-engineered authenticated endpoints.

## What is implemented

- Home dashboard with live threat, risk and incident posture.
- Threat Profile with six objectives: Extortion, Data Disclosure, Fraud, Sabotage, Resource Hijacking and Customer Targeting; includes draft, propose and approve workflow.
- Intelligent Risk Register: dense table, filters, saved-view writes, detail drawer, tags, threat-objective mapping, deterministic score suggestions with reasoning, remediation task/status, CSV export, comments and Jira/ServiceNow/Linear ticket records.
- Incident Register: SEV-1 to SEV-5 rubric, response and containment milestones, tags and objectives, deterministic severity suggestions with reasoning, comments and tickets, linked risks, plus a lesson-learned action that creates and links a new risk.
- Compliance: Charter, RAMP, CIRP and CISP document cards, proposed versions, approval and governed-section metadata.
- Board and CyberGov reporting: report records plus generated `.pptx` decks with executive summary, Threat Profile, risk/remediation view and incident timeline.
- Portfolio view with normalized risk-SLA, incident and governance metrics across demo companies.
- Settings: Wiz, CrowdStrike, HackerOne, WatchTowr, Jira, ServiceNow, Linear, Slack and Teams integration records; auto-score toggles; seven documented RBAC roles; notification and data-model views.
- In-app notifications and audit-log persistence.
- Demo user switcher for exercising RBAC without inventing a production identity provider.

## Architecture

- Next.js 16 App Router frontend plus Route Handlers as the serverless backend.
- Local development uses Node 22 built-in `node:sqlite` with a local SQLite database file under `data/`.
- Serverless production uses `TURSO_DATABASE_URL` and `TURSO_AUTH_TOKEN`; the adapter switches to `@tursodatabase/serverless/compat`, preserving SQLite and libSQL semantics.
- PptxGenJS provides live report deck export.

## Run locally

```bash
cp .env.example .env.local
npm install
npm run db:reset
npm run dev
```

Open `http://localhost:3000`. Use Node 22 or newer for the local `node:sqlite` adapter.

## Deploy serverlessly

Create a Turso or libSQL database, set `TURSO_DATABASE_URL` and `TURSO_AUTH_TOKEN` in the deployment environment, then deploy normally to a Next.js serverless host such as Vercel. On first access, the route layer creates the schema and seeds the demo organization if the database is empty.

## Important fidelity boundary

The private authenticated product was not publicly readable during this review. Exact RAMP and CIRP algorithms, prompts, integration payload mappings, report templates, authentication and authorization implementation, and internal business rules therefore cannot be reproduced exactly. The deterministic scoring functions in `lib/scoring.ts` are deliberately labeled demo approximations and remain editable and reviewable. Public screenshots were used as the visual baseline for navigation, registers, detail drawers, compliance cards and governance decks.

See `RESEARCH.md` for the page and feature inventory used to derive the build.
