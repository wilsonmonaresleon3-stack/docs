# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A documentation site built on [Mintlify](https://mintlify.com). There is no application code or build step — content is authored as MDX pages and rendered by Mintlify. Pushing to the default branch deploys to production automatically (via the Mintlify GitHub app).

## Commands

```bash
npm i -g mint          # Install the Mintlify CLI (requires Node.js 19+)
mint dev               # Preview locally at http://localhost:3000 (run from repo root, where docs.json lives)
mint dev --port 3333   # Preview on a custom port
mint broken-links      # Validate all internal links
mint update            # Update the CLI if the local preview drifts from production
```

There are no tests or linters. Validation means running `mint broken-links` and previewing with `mint dev`. If `mint dev` errors with a `sharp` module / unknown error, deleting `~/.mintlify` and rerunning usually fixes it.

## Architecture

- **`docs.json`** is the single source of truth for site structure, theme, and navigation. It is NOT auto-generated. **Any new page must be added to the `navigation` tree in `docs.json` or it won't appear in the sidebar.** Navigation is organized as `tabs` → `groups` → `pages`, where each page entry is a file path relative to the repo root, without the `.mdx` extension (e.g. `essentials/settings`).
- **Pages** are `.mdx` files with YAML frontmatter (`title`, `description`, optional `icon`). Content directories group pages by topic: `essentials/` (Mintlify authoring features), `agent-ready/` (AI/agent-facing docs), `ai-tools/` (per-tool setup), `api-reference/` (API docs).
- **API reference** pages are thin: an `.mdx` file with an `openapi` frontmatter field (e.g. `openapi: 'GET /plants'`) that pulls the operation from `api-reference/openapi.json`. To add/change endpoints, edit `openapi.json` and reference operations from MDX rather than hand-writing request/response docs.
- **`snippets/`** holds reusable MDX fragments imported into multiple pages to keep repeated content in sync (DRY).
- **Static assets** live in `images/` and `logo/`; referenced by absolute paths like `/images/checks-passed.png`.
- **`.mintignore`** excludes files from the build. Mintlify already ignores `.git`, `.github`, `node_modules`, `README.md`, `LICENSE`, `CONTRIBUTING.md`, etc. by default; this file adds `drafts/` and `*.draft.mdx`.

## Writing conventions

From `CONTRIBUTING.md` and `AGENTS.md`:

- Active voice and second person ("you", not "the user").
- One idea per sentence; lead with the goal.
- Sentence case for headings.
- Bold for UI elements (Click **Settings**); code formatting for file names, commands, paths.
- Use consistent terminology — don't alternate synonyms for the same concept.
- Mintlify components (`<Card>`, `<Columns>`, `<Steps>`, `<Accordion>`, `<Note>`, `<Info>`, `<Frame>`, etc.) are available in MDX; see `essentials/components.mdx` for usage.

For deeper Mintlify product knowledge (full component reference, configuration, writing standards), install the Mintlify skill: `npx skills add https://mintlify.com/docs`.

> Note: `AGENTS.md` is currently the uncustomized starter template (its Terminology and Content boundaries sections are empty placeholders). Treat it as a stub until project-specific terms are filled in.
