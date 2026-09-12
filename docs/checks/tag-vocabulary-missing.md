---
type: reference
title: tag-vocabulary-missing
description: A sub-bundle index using tags should declare a Tag vocabulary section.
---

# `tag-vocabulary-missing`

**Severity:** WARN · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

A sub-bundle's `index.md` (not the canonical root's) can declare a controlled vocabulary of
tags under a "Tag vocabulary" heading. If the bundle has at least one member document using
`tags`, but its index declares no such section at all, that's flagged.

## Why

Without a declared vocabulary, nothing constrains what tags mean in that bundle — two
documents could use slightly different spellings for the same concept (`getting-started` vs.
`getting_started`) and nothing would catch it.

## How to fix

Add a "Tag vocabulary" section to the sub-bundle's `index.md`, listing the allowed tags — for
example, as a bullet list of backtick-quoted tag names.

## Examples

**Incorrect** (`docs/guides/index.md`, where member documents already use `tags`):

```markdown
# Guides

- [Testing](/docs/guides/testing.md)
```

**Correct:**

```markdown
# Guides

- [Testing](/docs/guides/testing.md)

### Tag vocabulary

- `testing`
- `ci`
```
