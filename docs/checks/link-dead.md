---
type: reference
title: link-dead
description: Every internal link must resolve to a file that actually exists.
---

# `link-dead`

**Severity:** ERROR (WARN when the link is inside `log.md`) · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every absolute, repo-rooted internal link must resolve to a file that actually exists. A link
pointing at a path with nothing there fails this check — except inside `log.md`, where the
same finding is only a warning, not an error.

## Why

Everywhere but a log, a dead link means a reader following it hits a dead end right now, which
is always worth failing the run over. A log is different: it's a historical record, and an old
entry may legitimately reference something that's since been deleted — that's not a defect in
the entry itself, so it's flagged for attention rather than treated as a hard failure.

## How to fix

Repoint the link at the correct path, or remove it if the target no longer exists. Inside
`log.md`, the right fix is usually to unlink it rather than repoint it — the entry is a record
of what was true then, not a claim about what's true now.

## Examples

**Incorrect:**

```markdown
[testing guide](/docs/guides/testing-old.md)
```

(ERROR anywhere else; WARN if this line is in `log.md`)

**Correct:**

```markdown
[testing guide](/docs/guides/testing.md)
```
