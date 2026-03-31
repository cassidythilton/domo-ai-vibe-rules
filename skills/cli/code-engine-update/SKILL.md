---
name: code-engine-update
description: Update Domo Code Engine packages through CLI-driven versioned lifecycle workflows with compatibility checks, datatype contract safeguards, and manifest mapping drift synchronization. Use when an agent must update package code or create a new package version and keep app mappings aligned.
---

# Code Engine Package Update (CLI)

Use this skill for package update/version workflows and safe contract evolution.

## Intent

This skill covers:

- update existing package code
- create a new version on an existing package id
- detect and handle mapping drift between package and app manifest
- validate datatype contract stability

For app-runtime invocation code, use `skills/apps/code-engine/SKILL.md`.

## Primary Execution Path

Prefer `community-domo-cli`:

```bash
community-domo-cli --instance <instance> code-engine update-version <package_id> <version> --body-file payload.json
```

Fallback endpoint pattern:

```http
POST /api/codeengine/v2/packages
```

with `id` and `version` set in body for versioned update behavior.

## Update/New-Version Behavior

Use this decision model:

- no `packageId` -> create flow (handoff to `code-engine-create`)
- `packageId` only -> update current package context
- `packageId` + `version` -> create/update that explicit version

Always emit resulting `packageId` and `version` for downstream manifest sync.

## Contract Safeguards

Before update publish:

- compare existing function aliases vs proposed aliases
- compare parameter names/types/nullability
- compare output shape/type
- flag breaking changes (removed required params, incompatible type changes)

When breaking change is intentional, require explicit migration note in generated output.

## Drift Handling

After update:

- compare `manifest.json packagesMapping` against returned package id/version
- sync `packageId`, `version`, and parameter/output contracts when drift detected
- keep mapping aliases stable unless user requested rename

## Checklist

- [ ] Correct update path selected (existing vs versioned)
- [ ] Input/output datatype contracts validated
- [ ] Breaking changes flagged clearly
- [ ] Manifest `packagesMapping` drift checked and synchronized
- [ ] Output includes updated package id/version and contract summary
