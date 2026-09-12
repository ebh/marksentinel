---
type: reference
title: tag-unused
description: A declared tag should be used by at least one document in its bundle.
---

# `tag-unused`

**Severity:** WARN · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every tag a sub-bundle's index declares in its "Tag vocabulary" section should be used by at
least one member document. A declared tag nothing actually uses is flagged — on the index,
not any particular document.

## Why

A tag vocabulary is meant to describe reality, not aspiration. An unused entry is a promise
the bundle doesn't keep, and makes the vocabulary a less reliable guide to what's actually in
the bundle.

## How to fix

Either use the tag on a document it genuinely applies to, or remove it from the vocabulary.

## Examples

**Incorrect** (`docs/guides/index.md` declares a tag no document in the bundle uses):

```markdown
### Tag vocabulary

- `testing`
- `deployment`
```

**Correct** (remove the unused entry, or tag a relevant document with it):

```markdown
### Tag vocabulary

- `testing`
```
