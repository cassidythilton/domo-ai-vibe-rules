---
description: Always-on high-level Domo App Platform guardrails and skill routing.
alwaysApply: true
---

# Domo App Platform Core Rule

## Platform constraints (always apply)
- Domo custom apps are client-side only static apps.
- Do not use SSR patterns (`getServerSideProps`, Remix loaders/actions, SvelteKit server files, Nuxt server routes, `pages/api`, `app/api`, `"use server"`).
- If SSR is detected, stop and explicitly recommend client-side refactor (React + Vite) or Code Engine for server logic.
- **Exception — Embed host applications:** The embed skills (`programmatic-filters`, `edit-embed`) target the *host* application that embeds Domo content, not a Domo custom app. These require server-side code (API routes for OAuth token exchange, JWT signing) and use the Domo public API (`api.domo.com`) and Identity Broker — this is expected and correct. The client-only constraint does not apply to embed host apps.

## API boundary
- Use Domo App Platform APIs (`domo.js`, `@domoinc/query`, `@domoinc/toolkit`).
- Do not confuse with Domo public/product APIs unless explicitly requested.
- **Exception — Embed skills:** `programmatic-filters` and `edit-embed` use the Domo public API and Identity Broker by design. When these skills are active, public API usage (`api.domo.com/oauth/token`, `api.domo.com/v1/stories/embed/auth`, Identity Broker URLs) is expected.

## Build/routing non-negotiables
- Use relative asset base for Domo hosting (`base: './'`).
- Prefer `HashRouter` unless rewrite behavior is known and intentional.

## API index
- Data API
- AppDB
- AI Service Layer
- Code Engine
- Workflows
- Embed (Programmatic Filters, Dataset Switching, Edit Embed / Identity Broker)
- Files / Filesets / Groups / User / Task Center

## Skill routing
- New app build playbook -> `skills/playbooks/initial-build/SKILL.md`
- Dataset querying -> `skills/apps/dataset-query/SKILL.md`
- Data API overview -> `skills/apps/data-api/SKILL.md`
- domo.js usage -> `skills/apps/domo-js/SKILL.md`
- Toolkit usage -> `skills/apps/toolkit/SKILL.md`
- AppDB -> `skills/apps/appdb/SKILL.md`
- AI services -> `skills/apps/ai-service-layer/SKILL.md`
- Code Engine -> `skills/apps/code-engine/SKILL.md`
- Workflows -> `skills/apps/workflow/SKILL.md`
- SQL queries -> `skills/apps/sql-query/SKILL.md`
- Manifest wiring -> `skills/apps/manifest/SKILL.md`
- Build/publish -> `skills/apps/publish/SKILL.md`
- DA CLI -> `skills/apps/da-cli/SKILL.md`
- Performance -> `skills/apps/performance/SKILL.md`
- Migration from Lovable/v0 -> `skills/apps/migrate-lovable/SKILL.md`
- Migration from Google AI Studio -> `skills/apps/migrate-googleai/SKILL.md`
- Connector IDE -> `skills/connectors/connector-dev/SKILL.md`
- Programmatic embed filters / dataset switching -> `skills/domo-everywhere/programmatic-filters/SKILL.md`
- JS API filters -> `skills/domo-everywhere/jsapi-filters/SKILL.md`
- Edit embed / Identity Broker -> `skills/domo-everywhere/edit-embed/SKILL.md`
- Embed portal (full external-user portal) -> `skills/domo-everywhere/embed-portal/SKILL.md`
- HTML slide deck to PDF -> `skills/documents/html-deck/SKILL.md`
