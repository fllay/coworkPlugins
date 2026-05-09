---
name: Lint Knowledge Base
description: >
  Health-checks the knowledge base wiki at /Users/fa/Desktop/Projects/obsidianVault
  for quality issues: orphan pages, dead wikilinks, missing required sections,
  contradictions, and stale content. Trigger when the user says "lint the wiki",
  "check the knowledge base", "audit the wiki", "find problems", or "health check".
tools:
  - mcp__filesystem__read_file
  - mcp__filesystem__list_directory
  - mcp__filesystem__directory_tree
---

Run a full health check on the knowledge base.
Vault path: `/Users/fa/Desktop/Projects/obsidianVault`

## Step 1 — Read the Constitution
Read `SCHEMA.md` to understand the expected structure, page types, and rules.

## Step 2 — Inventory All Pages
Use mcp__filesystem__directory_tree to list everything in `wiki/`.
Read `INDEX.md` to get the expected page list.

## Step 3 — Run All Checks

### Check 1: Orphan pages
Pages in `wiki/` that are NOT listed in `INDEX.md`.

### Check 2: Index gaps
Entries in `INDEX.md` that point to files that do NOT exist in `wiki/`.

### Check 3: Dead wikilinks
Scan every page for `[[wikilink]]` references. Check each linked page actually exists.

### Check 4: Missing required sections
For each page, check it has all required sections for its type (defined in SCHEMA.md).
- Config page: Type/Default/Required/Config file block, What it does, Valid values, Example, When to change it
- Concept page: Key points, See also
- How-to page: Steps, Source
- Q&A page: Short answer, Source

### Check 5: Contradictions
Read all `wiki/config/` pages. Flag any where the same parameter appears with
different default values or incompatible descriptions.

### Check 6: Stale issue references
Scan pages for GitHub issue links marked as open. Note them for the user to verify.

## Step 4 — Report

Produce a structured report:

```
## Wiki Health Report — YYYY-MM-DD

### Summary
- Total pages: N
- Issues found: N (X critical, Y warnings)

### 🔴 Critical
- Orphan pages: list them
- Index gaps: list them
- Dead wikilinks: list them with which page they're in

### 🟡 Warnings
- Pages missing required sections: list page + missing section
- Potential contradictions: list the conflict
- Stale issue references: list them

### ✅ Healthy
- Everything else
```

Ask the user if they want to fix any issues automatically.
