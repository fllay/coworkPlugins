# kb-assistant

A personal knowledge base assistant for Claude Cowork. Ingests GitHub repos and web articles into an Obsidian vault structured around Andrej Karpathy's LLM Wiki pattern, then answers questions about the content.

## Skills

### `ingest`
**Trigger:** "ingest [url]", "add source [url]", "learn from [url]"

Fetches a GitHub repo (README, config files, source code, issues, discussions) or any web URL and creates structured wiki pages in your vault:
- `wiki/config/` — one page per named config parameter
- `wiki/concepts/` — core architecture concepts
- `wiki/how-to/` — step-by-step guides
- `wiki/qa/` — distilled Q&A from issues and discussions

### `update`
**Trigger:** "update the knowledge base", "refresh", "sync"

Re-fetches all previously ingested sources and merges new content into existing wiki pages.

### `lint`
**Trigger:** "lint the wiki", "health check", "audit"

Scans the vault for orphan pages, dead wikilinks, missing required sections, contradictions, and stale issue references.

## Setup

### 1. Required connectors
- **Filesystem** connector → point to your Obsidian vault path
- **GitHub MCP** → add to `claude_desktop_config.json` with a personal access token

### 2. Vault structure
The plugin expects this layout (created automatically on first ingest):
```
your-vault/
├── SCHEMA.md          ← wiki constitution
├── INDEX.md           ← content catalog
├── wiki/
│   ├── config/        ← one page per config parameter
│   ├── concepts/      ← core concepts
│   ├── how-to/        ← procedural guides
│   └── qa/            ← Q&A from issues/discussions
└── sources/
    └── _log.md        ← ingestion history
```

### 3. GitHub token (recommended)
Set `GITHUB_PERSONAL_ACCESS_TOKEN` in `claude_desktop_config.json` for:
- Full access to issues and discussions (5,000 req/hr vs 60 without token)
- Private repo access

## Installation

Upload `dist/kb-assistant.plugin` via **Customize → Personal plugins → "+"** in Claude desktop.

## Based on

[Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — persistent, compounding knowledge artifact maintained by an LLM.
