# Domo Agent Skills

> **A production-grade agent skill library for Domo custom app development.** Structured capabilities, workflows, and playbooks that let a solutions engineering team go AI-native — without reinventing the wheel on every build.

[![Skills](https://img.shields.io/badge/skills-18-6236FF?style=flat-square)](skills/)
[![Rules](https://img.shields.io/badge/rules-2-FF6C0C?style=flat-square)](rules/)
[![Cursor](https://img.shields.io/badge/Cursor-compatible-black?style=flat-square)](https://cursor.com)
[![Claude Code](https://img.shields.io/badge/Claude_Code-compatible-191919?style=flat-square)](https://claude.ai/code)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)

---

## Why This Exists

A solutions engineering team building on Domo's platform faces a recurring problem: Domo is broad. The same concept — "query data" — means something entirely different in a custom app vs. a Python script vs. an embedded dashboard. Without shared context, every developer re-learns the same gotchas, makes the same API mistakes, and produces inconsistent work.

This repo is the **shared skills layer** for going AI-native as a team:

- Skills encode hard-won platform knowledge so agents don't have to rediscover it
- Rules provide always-on guardrails that keep AI-assisted builds on the right track
- The taxonomy enforces consistency — every skill has a type, a feature area, and a name, making the library navigable as it grows

The goal is a compounding system: each new skill makes every subsequent build faster and more consistent. This is the foundation of a **GenAI control plane** — shared context and shared skills that don't silo.

> **See also:** [Helix](https://github.com/cassidythilton/helix-skills-graph) — an interactive knowledge graph that visualizes this skill library, its dependencies, and its operational health.

---

## Quick Start

```bash
# List all available skills
npx skills add cassidythilton/domo-ai-vibe-rules --list

# Install a single skill globally
npx skills add cassidythilton/domo-ai-vibe-rules --skill cap-apps-domo-js --global

# Install everything
npx skills add cassidythilton/domo-ai-vibe-rules --all --global
```

For new Domo app builds, start with `pb-apps-initial-build` — it routes to the correct skills in order.

---

## Taxonomy

Every skill follows the pattern `{type}-{feature}-{name}`. This makes a flat list navigable and avoids ambiguity as the library grows.

### Skill Types

| Prefix | Type | What It Is | Example |
|--------|------|-----------|---------|
| `cap-` | **Capability** | Atomic, single-topic skill. One API, one tool, one concept. | `cap-apps-appdb` |
| `wf-` | **Workflow** | Multi-step procedure composing several capabilities for a bounded job. | `wf-apps-migrate-lovable` |
| `pb-` | **Playbook** | End-to-end orchestration runbook for a full outcome. References capabilities and workflows in order. | `pb-apps-initial-build` |

### Feature Areas

| Segment | Scope |
|---------|-------|
| `apps-` | Domo App Platform custom apps |
| `de-` | Domo Everywhere (embedding) |
| `connector-` | Custom Connector IDE |

---

## Available Skills

### Playbooks (`pb-`)

- **`pb-apps-initial-build`** — Kickoff sequence for new Domo app builds; routes to the right rules and skills in order.

### Capabilities (`cap-`)

**App Platform**

| Skill | What It Covers |
|-------|---------------|
| `cap-apps-da-cli` | Domo CLI scaffolding, generation, and manifest instance workflows |
| `cap-apps-publish` | Build and publish flow (`npm run build`, `cd dist`, `domo publish`) |
| `cap-apps-domo-js` | `ryuu.js` usage, navigation/events, and import safety |
| `cap-apps-dataset-query` | `@domoinc/query` syntax and constraints |
| `cap-apps-data-api` | High-level data-access routing |
| `cap-apps-toolkit` | `@domoinc/toolkit` client usage and response handling |
| `cap-apps-appdb` | Toolkit-first AppDB CRUD/query patterns |
| `cap-apps-ai-service-layer` | Toolkit-first AI client usage and parsing |
| `cap-apps-code-engine` | Code Engine function invocation patterns and contracts |
| `cap-apps-workflow` | Workflow start/status patterns and input contracts |
| `cap-apps-manifest` | `manifest.json` mapping requirements and gotchas |
| `cap-apps-sql-query` | SqlClient raw SQL query patterns and response parsing |
| `cap-apps-performance` | Data query performance rules |

**Domo Everywhere (Embed)**

| Skill | What It Covers |
|-------|---------------|
| `cap-de-programmatic-filters` | Server-side programmatic filtering and dataset switching for embedded Domo dashboards |
| `cap-de-edit-embed` | Embedded edit experience via the Domo Identity Broker with JWT authentication and role-based access |
| `cap-de-jsapi-filters` | Client-side JS API filter methods for embedded Domo content |

**Connectors**

| Skill | What It Covers |
|-------|---------------|
| `cap-connector-dev` | Connector IDE auth/data processing patterns |

### Workflows (`wf-`)

| Skill | What It Covers |
|-------|---------------|
| `wf-apps-migrate-lovable` | Convert SSR-heavy generated apps to Domo-compatible client apps |
| `wf-apps-migrate-googleai` | Convert AI Studio-origin projects to Domo static deploy contract |

---

## Rules

Rules live in `rules/` and are **always-on guardrails** — not installable modules, but files you copy once into your project.

| File | What It Enforces |
|------|-----------------|
| `core-custom-apps-rule.md` | Platform-wide guardrails for Domo custom app development |
| `custom-app-gotchas.md` | Common mistakes and how to avoid them |

### Rules vs. Skills

- **Skills** are task-scoped. Install them for specific jobs, invoke them when needed.
- **Rules** are always active. Copy them once; they apply to every conversation.

---

## Setup

### Cursor

1. Open your app project in Cursor.
2. Ensure `.cursor/rules/` exists in the project root.
3. Copy `rules/core-custom-apps-rule.md` and `rules/custom-app-gotchas.md` into `.cursor/rules/`.

### Claude Code

1. Open your app project.
2. Create a `rules/` folder in the project root if it doesn't exist.
3. Copy both files from `rules/` into `rules/`.

### Ask your agent to do it

```
Please set up my Domo development environment:
1) Install skills from cassidythilton/domo-ai-vibe-rules using npx skills add.
2) Copy rules/core-custom-apps-rule.md and rules/custom-app-gotchas.md into my project rules location.
3) Confirm when done.
```

### If `npx skills add` is blocked

Some corporate environments block `npx` or GitHub access. Manually copy the `skills/<name>/SKILL.md` files you need into your local skills directory.

---

## Repository Structure

```
domo-ai-vibe-rules/
├── README.md
├── rules/
│   ├── core-custom-apps-rule.md    # Always-on platform guardrails
│   └── custom-app-gotchas.md       # Common mistakes reference
└── skills/
    ├── pb-apps-initial-build/
    │   └── SKILL.md
    ├── cap-apps-appdb/
    │   └── SKILL.md
    ├── cap-apps-code-engine/
    │   └── SKILL.md
    └── ...                         # One SKILL.md per skill
```

---

## Related Repos

| Repo | Relationship |
|------|-------------|
| [helix-skills-graph](https://github.com/cassidythilton/helix-skills-graph) | Interactive knowledge graph that visualizes this skill library — dependencies, categories, health metrics |

---

## Compatibility

Tool-agnostic by design. Works with Cursor, Claude Code, and any agent that supports markdown skill files. Does not rely on tool-specific preamble files in the root.

---

## License

MIT
