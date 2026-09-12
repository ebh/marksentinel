---
type: reference
title: tags-one-line
description: A document's `tags:` frontmatter value must be written as a single-line flow array.
---

# `tags-one-line`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

When a document's frontmatter has a `tags:` key, its value must be written as a single-line
flow sequence — `tags: [foo, bar, baz]` — no matter how long that line gets. A YAML block
sequence (one tag per line, each prefixed with `-`), or a flow sequence that wraps onto more
than one line, both fail this check. This only fires once `tags:` is present and is an array;
whether to add `tags:` at all is a separate, non-error recommendation.

## Why

Every tag in use across a bundle gets discovered with a plain, line-oriented
`grep -rh "^tags:" docs` — no YAML parser involved. That only works if `tags:` and its value
sit on one line. A block sequence or a wrapped flow sequence still parses as valid YAML, but
prints as a bare `tags:` under that grep, so its tags are effectively invisible to anyone
relying on it.

## How to fix

Rewrite the value as a flow sequence on the same line as the `tags:` key, however long the
line ends up:

```yaml
tags: [reference, getting-started, cli]
```

## Examples

**Incorrect — block sequence:**

```yaml
---
tags:
  - reference
  - getting-started
---
```

**Incorrect — flow sequence wrapped across lines:**

```yaml
---
tags: [reference,
  getting-started]
---
```

**Correct:**

```yaml
---
tags: [reference, getting-started]
---
```
