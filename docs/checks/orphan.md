---
type: reference
title: orphan
description: Every document should be reachable from within its own bundle.
---

# `orphan`

**Severity:** WARN · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every non-reserved document in a bundle must be reachable by following links starting from
somewhere within that same bundle — not necessarily directly from its own index, but from the
index, another document, or a chain of links starting there. A document nothing in its bundle
links to at all fails this check.

## Why

A reader who starts at a bundle's index and follows links will never stumble onto a document
that nothing points to — however carefully it's written, it might as well not exist to them.

## How to fix

Link to the document from somewhere in its bundle — usually its own index, or a related
document that should naturally mention it.

## Examples

**Incorrect** (`docs/advanced-config.md` exists, but nothing in `docs/` links to it).

**Correct:** add a link to it, for example from `docs/index.md` or a related guide:

```markdown
- [Advanced configuration](/docs/advanced-config.md)
```
