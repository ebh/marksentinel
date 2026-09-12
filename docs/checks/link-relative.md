---
type: reference
title: link-relative
description: Every internal link must be an absolute, repo-rooted path.
---

# `link-relative`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every internal Markdown link (anything that isn't an external URL) must be written as an
absolute, repo-rooted path starting with `/` — for example `/docs/guides/testing.md` — never a
relative path like `../testing.md` or `testing.md`. A bare `#anchor` link within the same
document is exempt from this check — see [`link-anchor`](link-anchor.md).

## Why

A relative link's meaning depends on where the linking document lives. Move either endpoint —
rename a directory, relocate a file — and a relative link silently breaks, with no signal at
either end. An absolute, repo-rooted path means the same thing no matter which document
contains it.

## How to fix

Rewrite the link target as an absolute path from the repo root, starting with `/`.

## Examples

**Incorrect:**

```markdown
[testing guide](../guides/testing.md)
[testing guide](testing.md)
```

**Correct:**

```markdown
[testing guide](/docs/guides/testing.md)
```
