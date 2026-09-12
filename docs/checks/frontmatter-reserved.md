---
type: reference
title: frontmatter-reserved
description: Reserved files carry no frontmatter, except the canonical root's index.md.
---

# `frontmatter-reserved`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Two opposite rules, depending on which reserved file this is:

- The **canonical root's `index.md`** (the top-level `docs/index.md`) is the one reserved file
  that must carry frontmatter — specifically an `okf_version` key with a non-empty value.
  Missing frontmatter entirely, or frontmatter without a usable `okf_version`, both fail.
- **Every other reserved file** — any other `index.md` (a sub-bundle's), or any `log.md`
  anywhere — must carry no frontmatter at all. Having a frontmatter block at all fails,
  regardless of what's in it.

## Why

`okf_version` marks which edition of the profile a bundle was written against, and only the
canonical root needs to declare it, once, for the whole repo. Every other reserved file has no
metadata of its own to carry — a reader shouldn't have to check whether one might.

## How to fix

For the canonical root's `docs/index.md`, add (or fix) an `okf_version` key. For any other
reserved file, delete the frontmatter block entirely.

## Examples

**Incorrect — canonical root `docs/index.md` with no `okf_version`:**

```markdown
---
type: guide
---

# Documentation
```

**Incorrect — a sub-bundle's `log.md` carrying frontmatter:**

```markdown
---
type: log
---

# Log

## 2026-09-12
```

**Correct — canonical root `docs/index.md`:**

```markdown
---
okf_version: "1.0"
---

# Documentation
```

**Correct — any other reserved file:**

```markdown
# Log

## 2026-09-12
```
