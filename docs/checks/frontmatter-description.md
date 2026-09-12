---
type: reference
title: frontmatter-description
description: A document's frontmatter should declare a `description` written as one full sentence.
---

# `frontmatter-description`

**Severity:** WARN · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Warns in two situations: `description` is missing or empty, or it's present but its trimmed
text doesn't end in `.`, `!`, or `?` — i.e. it doesn't read as a complete sentence.

## Why

`description` is what gets surfaced in an index entry or a search result; it's meant to be
read on its own, not as a fragment or a dangling keyword phrase. Ending punctuation is a
simple, mechanical proxy for "this is a real sentence," not just a label.

## How to fix

Add a `description` (or rewrite an existing one) as one complete sentence ending in a full
stop, exclamation mark, or question mark.

## Examples

**Incorrect:**

```yaml
---
type: guide
description: how to set up dev environment
---
```

**Correct:**

```yaml
---
type: guide
description: How to set up a local development environment.
---
```
