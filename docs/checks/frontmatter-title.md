---
type: reference
title: frontmatter-title
description: A document's frontmatter should declare a `title`.
---

# `frontmatter-title`

**Severity:** WARN · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Warns when a document's frontmatter has no `title` key, or its value is empty. Unlike `type`,
this is a recommendation, not a hard requirement — the run doesn't fail because of it, but
it's flagged.

## Why

A title is how a document gets referred to at a glance — in an index entry, a generated
listing, or a link's hover text. Without one, anything that renders a friendly document list
has to fall back to the filename or the H1 text, which isn't always as clear.

## How to fix

Add a `title` key with a short, descriptive value.

## Examples

**Incorrect:**

```yaml
---
type: guide
---
```

**Correct:**

```yaml
---
type: guide
title: Getting started
---
```
