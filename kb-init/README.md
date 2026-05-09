# kb-init

Autonomous Cowork agent that builds a complete knowledge base for any GitHub or GitLab repository in a single command. Runs the full pipeline without manual steps.

## Usage

In any Cowork space with this plugin enabled:

```
init knowledge base for https://github.com/owner/repo
```

With optional docs site:
```
build a kb for https://github.com/owner/repo docs: https://docs.example.com
```

## What it does automatically

| Step | Action |
|------|--------|
| 1 | Creates vault folder structure and SCHEMA.md |
| 2 | Ingests wiki from GitHub: README, config files, top 30 issues, discussions |
| 3 | Downloads all raw config example files (`.yml`, `.toml`, etc.) |
| 4 | Shallow clones repo → runs graphify AST on all source files |
| 5 | Generates `graph.html` (browser), `graph.graphml` (Gephi), graphify community wiki |
| 6 | Writes `CODE_ARCHITECTURE.md` — indexed by Obsidian Copilot |
| 7 | Adds `kb-graphify-<repo>` MCP server to Claude desktop config |

## After running

1. **Quit Claude (Cmd+Q) and reopen** → activates the new MCP server
2. **Re-index Obsidian Copilot** → picks up `CODE_ARCHITECTURE.md` and wiki pages
3. **Ask questions** in the Cowork space

## Requirements

- `kb-github` MCP configured with a GitHub personal access token
- Filesystem connector pointed at your Obsidian vault
- Python 3.11 available (auto-detects path)
- `graphifyy` pip package (auto-installed if missing)
- `git` available in PATH

## Output location

All outputs go into your vault:
```
your-vault/
├── CODE_ARCHITECTURE.md          ← Copilot-indexed architecture overview
├── wiki/                         ← structured knowledge pages
├── sources/
│   └── configs/                  ← raw config example files
└── graphify-out/
    └── <repo-name>/
        ├── graph.html            ← interactive source code graph
        ├── graph.graphml         ← full graph for Gephi
        ├── graph.json            ← graph data (for MCP queries)
        └── wiki/                 ← community articles
```

## Installation

Upload `dist/kb-init.plugin` via **Customize → Personal plugins → "+"** in Claude desktop.
