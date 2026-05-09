---
name: Ingest Source
description: >
  Fetches a GitHub repo (source code, config files, issues, discussions, docs) or
  any web URL and builds structured wiki pages in the knowledge base vault at
  /Users/fa/Desktop/Projects/obsidianVault. Trigger when the user says "ingest",
  "add source", "learn from", "add this repo", "add this link", or provides a
  GitHub or web URL to add to the knowledge base.
tools:
  - Read
  - Write
  - WebFetch
  - mcp__github__get_file_contents
  - mcp__github__list_directory
  - mcp__github__search_issues
  - mcp__github__get_issue
  - mcp__github__list_issues
  - mcp__filesystem__read_file
  - mcp__filesystem__write_file
  - mcp__filesystem__list_directory
  - mcp__filesystem__create_directory
---

The user wants to add a new source to the knowledge base.
Vault path: `/Users/fa/Desktop/Projects/obsidianVault`

## Step 1 — Identify Source Type

Classify the URL:
- **GitHub repo** (`github.com/owner/repo` without further path): full ingestion
- **GitHub docs repo** (repo name contains "doc", "wiki", or is a separate docs repo): focus on markdown files
- **Web URL** (anything else): fetch page content

## Step 2 — Fetch Content

### GitHub repo (full ingestion):

1. Read `README.md` via `mcp__github__get_file_contents`
2. Call `mcp__github__list_directory` on the root to map the repo structure
3. **Config files**: find and read every file matching `*.yaml`, `*.yml`, `*.toml`, `*.json` (excluding `package.json`, `package-lock.json`), `*.conf`, `*.ini`, `*.env.example`, `*.cfg` — prioritize files in `config/`, `conf/`, `settings/`, or root
4. **Docs**: read all `.md` files in `docs/`, `doc/`, `documentation/`, or a `wiki/` folder
5. **Issues**: call `mcp__github__list_issues` with `state=all`, sorted by `comments` descending — read the top 30 most-discussed issues using `mcp__github__get_issue`
6. **Source code structure**: read key files that define configuration schemas or defaults (e.g., files named `defaults.*`, `schema.*`, `config.*`, `settings.*`)

### Web URL:

1. Use `WebFetch` to retrieve the page
2. Extract config parameters, setup steps, and any troubleshooting content

## Step 3 — Analyze & Plan Wiki Pages

From all fetched content, identify what pages to create or update:

| Content found | Wiki page location |
|---|---|
| A named config parameter | `wiki/config/<param-name>.md` |
| A core concept or term | `wiki/concepts/<concept>.md` |
| A setup/install procedure | `wiki/how-to/<task>.md` |
| A resolved issue or common question | `wiki/qa/<topic>.md` |

Use kebab-case filenames. One page per distinct config parameter — do not bundle multiple parameters into one page.

## Step 4 — Write Wiki Pages

For every identified page:

1. Check if it already exists via `mcp__filesystem__read_file`. If it does, **merge** new information — append new examples, new issue references, updated defaults. Never overwrite existing content wholesale.
2. If it doesn't exist, create it using the templates in `references/wiki-templates.md`.
3. Add Obsidian cross-reference links (`[[page-name]]`) to related pages.

Create any missing directories first with `mcp__filesystem__create_directory`.

## Step 5 — Update INDEX.md

Read or create `/Users/fa/Desktop/Projects/obsidianVault/INDEX.md`.

Maintain this structure:
```markdown
# Knowledge Base Index

## Config Parameters
- [[param-name]] — one-line description

## Concepts
- [[concept-name]] — one-line description

## How-To Guides
- [[how-to-task]] — one-line description

## Q&A
- [[qa-topic]] — one-line description
```

Add all new pages under the correct section. Keep entries alphabetical within each section.

## Step 6 — Log the Source

Append to `/Users/fa/Desktop/Projects/obsidianVault/sources/_log.md`:
```
- [YYYY-MM-DD] <url> — <brief description: what was ingested, how many pages created/updated>
```

Create the file and `sources/` directory if they don't exist.

## Step 7 — Report to User

Tell the user:
- Source ingested
- Number of wiki pages created vs. updated
- List of config parameters now documented (if any)
- Suggest a follow-up: "Ask me anything about [topic], or `/update` to refresh later."
