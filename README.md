# Domo AI Vibe Rules

> **Make AI coding assistants understand Domo** — so you can build custom apps faster.

---

## What is this?

When you use AI coding tools like [Cursor](https://cursor.sh/) or [Claude Code](https://claude.ai), they don't know anything about Domo's APIs. This repo gives them that knowledge.

**The problem:** You prototype an app in Google AI Studio or Lovable, then move it to your local machine to continue development. But now your AI assistant doesn't know how to:
- Connect to Domo datasets
- Use AppDB for storage
- Call Code Engine functions
- Trigger workflows
- ...or any other Domo-specific stuff

**The solution:** Copy these rules files into your project. Your AI assistant will now understand Domo.

---

## Who is this for?

This is for anyone who hasn't yet started crafting their own Cursor or Claude Code rules. It's a solid starting point that makes it much more likely the AI will generate code that correctly integrates with Domo's APIs — saving you time debugging and fixing hallucinated code.

---

## 🚀 Quick Start (Claude Code)

### Step 1: Copy CLAUDE.md to your project

Copy `CLAUDE.md` to your project's **root folder**:

```
my-domo-app/
├── CLAUDE.md        ← Put it here
├── public/
├── src/
└── ...
```

This gives Claude Code the core Domo App Platform knowledge.

### Step 2: Add API-specific files as needed

When working on specific features beyond the Domo App Platform, **add the relevant .md file to your chat context**:

- Need to query datasets? → Add `domo-data-api.md` to context
- Need to store app data? → Add `domo-appdb.md` to context
- Need AI features? → Add `domo-ai-endpoints.md` to context
- Need Code Engine? → Add `domo-code-engine.md` to context
- Need Workflows? → Add `domo-workflow.md` to context
- Migrating from Google AI Studio? → Add `google-ai-studio-to-domo.md` to context
- Building custom connectors? → Add `domo-custom-connector-ide.md` to context

**You don't need to copy content from these files into CLAUDE.md** - just add the specific file to your conversation when you need it. This keeps context lean and focused.

See the table in `CLAUDE.md` for a complete guide on when to include each file.

---

## 🚀 Quick Start (Cursor)

### Step 1: Download the rules

Download these files from this repo:
- `.cursor/rules/*.mdc` — Canonical rules (recommended)
- `CLAUDE.md` — Claude-oriented entrypoint
- `.cursorrules` — Backward compatibility only (deprecated)

### Step 2: Add to your project

**Option A: Recommended approach**

Create `.cursor/rules/` in your project and copy the modular rule files:

```
my-domo-app/
├── .cursor/
│   └── rules/
│       ├── 01-overview.mdc
│       ├── 02-ryuu-js.mdc
│       ├── 03-query.mdc
│       ├── 04-toolkit.mdc
│       └── ...
├── public/
├── src/
└── ...
```

**Option B: Backward compatibility**

If needed for legacy setup, place `.cursorrules` in your project root:

```
my-domo-app/
├── .cursorrules     ← Backward compatibility only
├── public/
├── src/
└── ...
```

### Step 3: Configure when rules apply

This is important for keeping AI responses accurate:

| Rule | Setting | Why |
|:-----|:--------|:----|
| **`01-overview.mdc`** | **Always** | Core Domo platform constraints and architecture |
| **API modules** (`03-query.mdc`, `04-toolkit.mdc`, etc.) | **Agent-decided** | Keep context focused and avoid contradictory guidance |

> **Why does this matter?** If you inject AppDB documentation into context when your app doesn't use AppDB, you're adding irrelevant information. This ambiguity is when AI starts to hallucinate — it might suggest using AppDB when you don't need it.

### Step 4: Start coding!

Open your project in Cursor and start chatting with the AI. It will now understand Domo.

---

## 🔄 Coming from a Prototyping Tool?

If you built your initial prototype elsewhere, here are some things to watch out for:

### From Google AI Studio

Google AI Studio automatically builds and deploys your code to Cloud Run — you never deal with a build process. When you move to local development, you'll need to set up a proper build pipeline yourself.

**What to do:** Use the `google-ai-studio-to-domo.md` rules file. It will help the AI:
- Detect that your project came from AI Studio
- Set up Vite (or another bundler) for local builds
- Configure the project to output static files that Domo can host
- Handle the transition from Cloud Run to static deployment

### From Lovable

Lovable sometimes generates projects with **server-side rendering (SSR)**. This is a problem because Domo custom apps are **client-side only** — there's no server to render your pages.

**Signs your project uses SSR:**
- Next.js with `getServerSideProps` or `getStaticProps`
- Remix loaders
- SvelteKit with server routes
- Files in `pages/api/` or `app/api/`
- References to `process.env` that expect server-side secrets

**What to do:** If your project has SSR, you'll need to refactor to a pure client-side app (React with Vite is recommended). The AI will detect this and warn you.

---

## 📁 What's in this repo?

### Main Rules Files

| File | What it's for |
|:-----|:--------------|
| **`.cursor/rules/*.mdc`** | Canonical Cursor rules (recommended) |
| **`.cursorrules`** | Deprecated compatibility file |
| **`CLAUDE.md`** | Copy this to your project for Claude Code |

### API Reference Files

**For Claude Code users:** Add these files to your chat context when needed (don't copy into CLAUDE.md)

**For Cursor users:** use `.cursor/rules/*.mdc` as canonical.

| File | When you need it |
|:-----|:-----------------|
| `domo-data-api.md` | Toolkit/query-first data access reference |
| `domo-appdb.md` | Toolkit-first AppDB reference |
| `domo-ai-endpoints.md` | Toolkit-first AI reference |
| `domo-code-engine.md` | Toolkit-first Code Engine reference |
| `domo-workflow.md` | Toolkit-first Workflow reference |

Legacy endpoint-first versions of these docs are preserved under `archive/legacy-rules/`.

### Other Files

| File | What it's for |
|:-----|:--------------|
| `google-ai-studio-to-domo.md` | Migrating a Google AI Studio project to Domo |

---

## ⚠️ Important Note About Cursor

Cursor's rules system has changed several times in the past year. Current recommendation:

- **`.cursor/rules/`** — Canonical approach using modular `.mdc` files
- **`.cursorrules`** — Deprecated compatibility only
- **`AGENTS.md`** — Alternate format depending on workflow

**Our recommendation:** Start with `.cursor/rules/*.mdc`. Use `.cursorrules` only for backward compatibility.

---

## 📚 Official Domo Documentation

### Getting Started

👉 **[Local Development with Domo CLI](https://developer.domo.com/portal/c8adeafafc236-local-development-with-domo-cli)** — Start here! Quick guide to local custom app development.

### App Framework API Docs

These are the official docs for each API (as of January 2026):

| API | Documentation Link |
|:----|:-------------------|
| AI Service Layer | [View Docs](https://developer.domo.com/portal/wjqiqhsvpadon-ai-service-layer-api-ai-pro-assets-images-pro-png) |
| AppDB | [View Docs](https://developer.domo.com/portal/1l1fm2g0sfm69-app-db-api) |
| Code Engine | [View Docs](https://developer.domo.com/portal/p48phjy7wwtw8-code-engine-api) |
| Data API | [View Docs](https://developer.domo.com/portal/8s3y9eldnjq8d-data-api) |
| File Set (Beta) | [View Docs](https://developer.domo.com/portal/7e8654dedb1c8-file-set-api-beta) |
| Groups | [View Docs](https://developer.domo.com/portal/2hwa98wx7kdm4-groups-api) |
| Task Center | [View Docs](https://developer.domo.com/portal/k2vv2vir3c8ry-task-center-api) |
| User | [View Docs](https://developer.domo.com/portal/n7f7swo7h29wg-user-api) |
| Workflows | [View Docs](https://developer.domo.com/portal/1ay1akbc787jg-workflows-api) |

---

## 💡 Tips for Success

1. **Use canonical modules** — Start with `01-overview.mdc`, then add API modules as needed.

2. **Add project-specific notes** — There's a section at the bottom of the rules file for your own notes. Use it!

3. **Keep the AI focused** — Prefer `@domoinc/query` + `@domoinc/toolkit` patterns. Use raw endpoint calls only as explicit fallback.

4. **Don't forget `thumbnail.png`** — Every Domo app needs a `thumbnail.png` file alongside the `manifest.json`.

---

## 🤝 Contributing

Found an error? Have improvements? PRs welcome!

---

<p align="center">
  <i>Built to help Domo developers move faster.</i>
</p>
