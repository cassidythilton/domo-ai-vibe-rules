---
name: code-engine-create
description: Create Domo Code Engine packages from CLI workflows with deterministic payload contracts, automatic function parameter datatype mapping, and manifest packagesMapping follow-up guidance. Use when an agent must create a new package/versioned package container rather than only invoke an existing function from app runtime code.
---

# Code Engine Package Create (CLI)

Use this skill for package creation workflows, payload generation, and post-create mapping sync.

## Intent

This skill covers lifecycle operations that are out of scope for app-runtime invocation skills:

- create new package shell
- publish first package content payload
- infer and normalize function input/output datatypes
- update app `manifest.json` `packagesMapping` entries after create

For in-app function invocation patterns, use `skills/apps/code-engine/SKILL.md`.

## Primary Execution Path

Prefer `community-domo-cli`:

```bash
community-domo-cli --instance <instance> code-engine create-package --body-file payload.json
```

Fallback endpoint when CLI path is unavailable:

```http
POST /api/codeengine/v2/packages
```

## Required Payload Shape

```json
{
  "name": "My Package",
  "description": "Optional",
  "code": "// JS source",
  "environment": "LAMBDA",
  "language": "JAVASCRIPT",
  "manifest": {
    "functions": [
      {
        "name": "myFunction",
        "displayName": "My Function",
        "description": "",
        "inputs": [],
        "parameters": [],
        "output": {}
      }
    ],
    "configuration": {
      "accountsMapping": []
    }
  }
}
```

## Datatype Mapping Rules (Auto-Map)

When generating `manifest.functions[].inputs/parameters/output`, apply these defaults:

- `payload`, `params`, `config`, `data`, `body`, `options` => `object`
- names indicating counts/limits/offsets => `decimal`
- boolean-like names (`is*`, `has*`, `enabled`, `required`) => `boolean`
- identifiers/text fields (`*id`, `name`, `query`, `message`) => `text`
- unknowns => `text`
- output default => `object`

Use the same type value for `type` and `dataType` when both fields are present.

## Post-Create Manifest Follow-up

After successful create, update app `manifest.json` `packagesMapping`:

- set `packageId` to created package id
- set `version` to returned version (or explicit target version)
- ensure each mapped function has matching `parameters` and `output`

If package ID/version changes, treat existing mapping as drift and sync immediately unless user requests otherwise.

## Checklist

- [ ] CLI-first create attempted
- [ ] Endpoint fallback documented if CLI unavailable
- [ ] Payload contains `manifest.functions`
- [ ] Datatype mapping applied to each function input/output
- [ ] `packagesMapping` updated in manifest after create
- [ ] Result includes package id/version for downstream steps
