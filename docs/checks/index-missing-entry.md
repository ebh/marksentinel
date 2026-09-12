---
type: reference
title: index-missing-entry
description: An index.md must link to every document and child sub-bundle in its own bundle.
---

# `index-missing-entry`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every `index.md` — the canonical root's and every sub-bundle's — must link to every
non-reserved document that lives directly in its own directory, plus the `index.md` of every
*immediate* child sub-bundle, one directory level down, wherever it sits in the tree. It does
not need to link a grandchild sub-bundle directly — that one is reached by following the chain
through its immediate parent instead. Any expected document or child index missing from the
links fails this check.

## Why

An index is how a reader — or a crawler — discovers what a bundle contains. A document the
index doesn't mention might as well not exist to anyone who starts there.

## How to fix

Add a link to the missing document or child sub-bundle index.

## Examples

**Incorrect** (`docs/testing.md` and `docs/guides/index.md` also exist, but neither is
linked):

```markdown
# Documentation

- [Getting started](/docs/getting-started.md)
```

**Correct:**

```markdown
# Documentation

- [Getting started](/docs/getting-started.md)
- [Testing](/docs/testing.md)
- [Guides](/docs/guides/index.md)
```
