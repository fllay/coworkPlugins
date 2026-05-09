# Wiki Page Templates

Use these templates when creating new wiki pages.
Always fill in every field — use "unknown" or "see source" if not found in the source material.

---

## Config Parameter Page
**File:** `wiki/config/<parameter-name>.md`

```markdown
# `<parameter_name>`

**Type:** <string | int | float | bool | list | map>
**Default:** <value, or "none / required">
**Required:** Yes / No
**Config file:** <filename and section, e.g. `config.yaml > server`>

## What it does
<1–2 sentence plain-language description>

## Valid values
<range, list of options, or format description>

## Examples

### Minimal
```yaml
parameter_name: value
```

### Recommended for typical use
```yaml
parameter_name: value  # explain why
```

## When to change it
<practical guidance — what scenarios require changing this from the default>

## Common mistakes
<from issues/discussions — what goes wrong if misconfigured>

## Related issues / discussions
- [#123 Title](url) — brief note

## Related parameters
- [[other-param]] — how they interact
```

---

## Concept Page
**File:** `wiki/concepts/<concept>.md`

```markdown
# <Concept Name>

<1 paragraph explanation>

## Key points
- ...
- ...

## How it relates to configuration
<which config parameters control this>

## See also
- [[related-concept]]
- [[config-param]]
```

---

## How-To Guide
**File:** `wiki/how-to/<task>.md`

```markdown
# How to: <task>

## When to use this
<scenario / use case>

## Prerequisites
- ...

## Steps
1. ...
2. ...
3. ...

## Verification
<how to confirm it worked>

## Troubleshooting
<common issues and fixes, sourced from GitHub issues>

## Source
<link to original issue, doc, or discussion>
```

---

## Q&A Entry
**File:** `wiki/qa/<topic>.md`

```markdown
# <Question as asked in the issue/discussion>

**Short answer:** <one sentence>

## Full explanation
<detailed answer>

## Config impact
<which parameters to change, if any — link to [[config-param]] pages>

## Source
- [#123 Issue title](url)
- [Discussion: Topic](url)
```
