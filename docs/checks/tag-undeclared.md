---
type: reference
title: tag-undeclared
description: A document's tags must all appear in its bundle's declared tag vocabulary.
---

# `tag-undeclared`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Once a sub-bundle's index declares a tag vocabulary (see
[`tag-vocabulary-missing`](tag-vocabulary-missing.md)), every tag any member document in that
bundle uses in its own `tags:` frontmatter must appear in that vocabulary. A tag used but not
declared fails this check — on the document that used it, not the index.

## Why

A declared vocabulary is only useful if it's actually closed. An undeclared tag slipping
through breaks tag-based discovery the same way an inconsistently spelled tag would, just less
visibly — nothing tells a reader that `ci-pipeline` and `ci` were meant to be the same thing.

## How to fix

Either add the tag to the sub-bundle index's "Tag vocabulary" section, or change the document
to use a tag spelling that's already declared.

## Examples

**Incorrect** (`docs/guides/testing.md`, where `docs/guides/index.md`'s vocabulary only
declares `testing` and `ci`):

```yaml
---
type: guide
tags: [testing, ci-pipeline]
---
```

**Correct** (use an already-declared spelling, or add the new tag to the vocabulary):

```yaml
---
type: guide
tags: [testing, ci]
---
```
