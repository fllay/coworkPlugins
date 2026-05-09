---
name: Update Knowledge Base
description: >
  Re-fetches all previously ingested sources and refreshes the wiki pages in
  /Users/fa/Desktop/Projects/obsidianVault with new content (new issues, updated
  docs, changed config defaults). Trigger when the user says "update the knowledge
  base", "update wiki", "refresh", "re-fetch sources", "check for updates", or
  "sync the knowledge base".
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

Re-ingest all previously logged sources and refresh the wiki.

## Steps

1. Read `/Users/fa/Desktop/Projects/obsidianVault/sources/_log.md` to get the full list of previously ingested URLs.

2. For each URL in the log, run the full ingest process (same logic as the Ingest Source skill).

3. **Merging rules when updating existing pages:**
   - Add new examples, new issue references, and updated defaults
   - If a default value has changed, update it and note the version where it changed
   - Do not delete existing content unless it is explicitly contradicted by new source material
   - Append rather than replace

4. Update each source's entry in `_log.md` with a refresh timestamp:
   ```
   - [original-date → refreshed YYYY-MM-DD] <url> — <description>
   ```

## After updating

Tell the user:
- How many sources were refreshed
- How many pages were updated vs. unchanged
- Any new pages discovered (new config params or concepts found)
- Any notable changes (e.g., "default for `batch_size` changed from 32 to 64")
