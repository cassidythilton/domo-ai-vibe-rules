# Toolkit Migration Audit

This document records the migration from legacy endpoint-first docs (`domo.get/post/put/delete`) to toolkit/query-first guidance.

## Scope Reviewed

- Legacy root docs (pre-migration snapshots now in `archive/legacy-rules/`):
  - `domo-data-api.md`
  - `domo-appdb.md`
  - `domo-ai-endpoints.md`
  - `domo-code-engine.md`
  - `domo-workflow.md`
- Canonical modular rules:
  - `.cursor/rules/03-query.mdc`
  - `.cursor/rules/04-toolkit.mdc`
  - `.cursor/rules/09-gotchas.mdc`
  - `.cursor/rules/10-performance-optimization.mdc`

## Overlap Matrix

| Area | Legacy coverage | Toolkit/query coverage | Result |
|---|---|---|---|
| Data querying | Raw `/data/v1` and `/sql/v1` endpoint examples | Full `@domoinc/query` chainable docs + performance constraints | Canonicalized to Query-first |
| AppDB CRUD | Full endpoint CRUD + bulk patterns | `AppDBClient.DocumentsClient` + Mongo-style query/update operators | Canonicalized to AppDBClient-first |
| AI generation | Endpoint examples with `choices[0].output` | `AIClient` methods, snake_case, response-shape caveats | Canonicalized to AIClient-first |
| Code Engine | Endpoint invocation + input/output notes | `CodeEngineClient.execute` + manifest mapping | Canonicalized to CodeEngineClient-first |
| Workflow | Endpoint trigger + response + manifest mapping | `WorkflowClient` methods + instance checks | Canonicalized to WorkflowClient-first |

## Legacy-Only Content Not Fully Represented Yet

These were present in legacy docs and are not yet equally detailed in canonical toolkit/query docs:

1. **AI image-to-text (OCR) endpoint walkthrough**
   - Legacy had explicit `/domo/ai/v1/image/text` payload examples with base64 prefix stripping.
   - Toolkit docs currently emphasize text-oriented `AIClient` methods.
   - Recommendation: add a toolkit-compatible OCR section (or explicit fallback note) in `.cursor/rules/04-toolkit.mdc`.

2. **Detailed raw endpoint query-parameter grammar**
   - Legacy data doc covered `fields/filter/groupby/orderby/limit/offset` URL usage directly.
   - Query docs intentionally replaced this with query-builder patterns.
   - Recommendation: keep this omitted from canonical docs unless a specific raw-endpoint fallback section is requested.

3. **Workflow “no data return” hard rule**
   - Legacy stated workflows return success/failure only.
   - Current toolkit docs focus on `WorkflowClient` instance/status and are less absolute.
   - Recommendation: keep current toolkit framing (less misleading), do not restore hard rule.

4. **Code Engine output-shape debugging narrative**
   - Legacy instructed runtime inspection of unknown output structures.
   - Toolkit examples use `response.body.output`.
   - Recommendation: optionally add one “inspect unknown output contracts” troubleshooting snippet.

## Toolkit/Query-Only Content Added

1. **Typed service clients (`@domoinc/toolkit`)**
   - `AppDBClient`, `AIClient`, `IdentityClient`, `SqlClient`, `CodeEngineClient`, `WorkflowClient`, etc.

2. **Query-first architecture**
   - Performance-centered `@domoinc/query` usage with server-side aggregation guidance.

3. **Critical operational gotchas**
   - `.aggregate()` non-working behavior (`DA0057`)
   - groupBy constraints
   - dataset mapping `fields: []` requirement
   - toolkit client caveats (snake_case, response wrapper)

## Contradictions Found and Resolved

1. **AI response shape contradiction**
   - Conflict: “responses are in `choices[0].output`” vs toolkit’s `response.data` behavior.
   - Resolution: canonical parse rule now uses:
     - `const body = response.data || response.body || response`
     - `const output = body.output || body.choices?.[0]?.output`

2. **Endpoint-first vs client-wrapper guidance**
   - Conflict: root docs taught direct REST patterns as primary while toolkit docs promoted wrappers.
   - Resolution: root API docs are now toolkit/query-first mirrors; endpoint-first docs moved to archive.

3. **Cursor canonical source ambiguity**
   - Conflict: README previously favored `.cursorrules` simplicity while `.mdc` is modern source.
   - Resolution: README now treats `.cursor/rules/*.mdc` as canonical and `.cursorrules` as compatibility.

## Files Changed in This Migration

- Archived legacy snapshots:
  - `archive/legacy-rules/domo-data-api.md`
  - `archive/legacy-rules/domo-appdb.md`
  - `archive/legacy-rules/domo-ai-endpoints.md`
  - `archive/legacy-rules/domo-code-engine.md`
  - `archive/legacy-rules/domo-workflow.md`
- Canonicalized root docs:
  - `domo-data-api.md`
  - `domo-appdb.md`
  - `domo-ai-endpoints.md`
  - `domo-code-engine.md`
  - `domo-workflow.md`
- Alignment updates:
  - `.cursor/rules/04-toolkit.mdc`
  - `.cursor/rules/09-gotchas.mdc`
  - `.cursorrules`
  - `README.md`
  - `CLAUDE.md`

## Recommended Rule Setup (Final)

Use **split modular files**, not a single large rules file.

### Why split wins

- Reduces context pollution (agent loads only relevant topics).
- Easier to maintain and review per API domain.
- Lower risk of contradictory guidance across unrelated concerns.

### Canonical hierarchy

1. **Primary:** `.cursor/rules/*.mdc`
2. **Companion:** root `domo-*.md` (toolkit/query-first mirror docs)
3. **Compatibility:** `.cursorrules` (deprecated pointer + fallback)
4. **Historical reference:** `archive/legacy-rules/*.md` (non-canonical)
