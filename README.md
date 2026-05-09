# Cowork Plugins

A collection of Claude Cowork plugins for building and querying personal knowledge bases from GitHub repositories and documentation.

Built around [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): a persistent, compounding knowledge artifact maintained by an LLM.

---

## Plugins

### [`kb-init`](./kb-init/) — Knowledge Base Builder
**One command to build a full knowledge base for any GitHub/GitLab repo.**

```
init knowledge base for https://github.com/owner/repo
```

Runs automatically: wiki ingestion → source code graphify analysis → architecture doc → MCP server setup.

→ [Install `kb-init`](./dist/kb-init.plugin) | [Documentation](./kb-init/README.md)

---

### [`kb-assistant`](./kb-assistant/) — Knowledge Base Assistant
**Chat with your knowledge base. Ingest, update, and audit wiki pages.**

```
ingest https://github.com/owner/repo
update the knowledge base
lint the wiki
```

Three skills: `ingest` (add sources), `update` (refresh), `lint` (health check).

→ [Install `kb-assistant`](./dist/kb-assistant.plugin) | [Documentation](./kb-assistant/README.md)

---

## Quick start

### Recommended workflow

1. **Install both plugins** from `dist/` via Customize → Personal plugins → "+"
2. **Set up connectors:**
   - Filesystem connector → your Obsidian vault path
   - Add GitHub MCP to `claude_desktop_config.json` (see below)
3. **Open a Cowork space** and run:
   ```
   init knowledge base for https://github.com/your-project
   ```
4. **Quit and reopen Claude** to activate the MCP server
5. **Re-index Obsidian Copilot** to pick up the new content
6. **Ask questions** in the Cowork space

### GitHub MCP setup

Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "kb-github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here"
      }
    }
  }
}
```

Get a token at: GitHub → Settings → Developer settings → Personal access tokens
Required scopes: `repo`, `read:discussion`

---

## How it works

```
GitHub repo / docs URL
        │
        ▼
  kb-init agent
  ┌─────────────────────────────────────────────┐
  │  Step 1: Vault structure                     │
  │  Step 2: Ingest wiki (issues, docs, config)  │
  │  Step 3: Save raw config example files       │
  │  Step 4: Clone + graphify AST (source graph) │
  │  Step 5: Generate graph HTML + wiki          │
  │  Step 6: Write CODE_ARCHITECTURE.md          │
  │  Step 7: Add graphify MCP to Claude config   │
  └─────────────────────────────────────────────┘
        │
        ▼
  Obsidian vault
  ┌─────────────────────────────────────────────┐
  │  wiki/           ← structured knowledge      │
  │  sources/configs ← raw config examples       │
  │  graphify-out/   ← source code graphs        │
  │  CODE_ARCHITECTURE.md ← Copilot-indexed      │
  └─────────────────────────────────────────────┘
        │
        ▼
  Query via:
  • Cowork chat (graphify MCP + filesystem MCP)
  • Obsidian Copilot (indexes wiki + architecture)
  • Obsidian graph view (wiki wikilinks)
  • Gephi (full source code graph)
```

---

## Architecture: Karpathy's LLM Wiki pattern

These plugins implement the three-layer pattern:

| Layer | What it is | Where it lives |
|-------|-----------|----------------|
| **Raw sources** | GitHub repo, docs URLs, issues | `sources/_log.md` (references) |
| **The wiki** | LLM-synthesized markdown pages | `wiki/` |
| **The schema** | Constitution defining structure | `SCHEMA.md` |

Three operations:
- **Ingest** — process a source, create/update wiki pages
- **Query** — answer questions using wiki pages + graph
- **Lint** — health-check wiki for stale/broken content

---

## Requirements

- Claude desktop app with Cowork
- Python 3.11+ (for graphify)
- `git` in PATH
- `graphifyy` pip package (auto-installed by kb-init)
- GitHub personal access token (for issues/discussions)

---

## Vault structure created by these plugins

```
your-vault/
├── SCHEMA.md                    ← wiki constitution
├── INDEX.md                     ← master content index
├── CODE_ARCHITECTURE.md         ← source code overview (Copilot-indexed)
├── wiki/
│   ├── config/                  ← one page per config parameter
│   ├── concepts/                ← architecture concepts
│   ├── how-to/                  ← step-by-step guides
│   └── qa/                      ← Q&A from issues/discussions
├── sources/
│   ├── _log.md                  ← ingestion history
│   └── configs/                 ← raw config example files
└── graphify-out/
    └── <repo-name>/
        ├── graph.html           ← interactive graph (browser)
        ├── graph.graphml        ← full graph (Gephi/yEd)
        ├── graph.json           ← graph data (MCP queryable)
        └── wiki/                ← community articles
```
