---
type: reference
title: frontmatter-tags
description: A document's frontmatter should declare `tags` as an array.
---

# `frontmatter-tags`

**Severity:** WARN (missing), ERROR (present but not an array) · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Two distinct findings share this check name:

- **WARN** when `tags` is absent altogether — adding tags is a recommendation, not required.
- **ERROR** when `tags` is present but its value isn't an array at all (a bare string, a
  number, a nested mapping).

An empty array (`tags: []`) is valid and produces neither finding.

## Why

Tags are how documents get grouped and discovered across a bundle. Omitting them entirely is
a missed opportunity, but writing a non-array value breaks every piece of tooling downstream
that expects to iterate over a list — hence the difference in severity between "you didn't add
any" (WARN) and "you wrote something nothing can use as a list" (ERROR).

## How to fix

Add `tags: []` (or with real values) if it's missing. If it's present, make sure the value is
a YAML sequence, not a scalar. (See [`tags-one-line`](tags-one-line.md) for how that sequence
must be laid out.)

## Examples

**Incorrect — missing (WARN):**

```yaml
---
type: guide
---
```

**Incorrect — not an array (ERROR):**

```yaml
---
type: guide
tags: reference
---
```

**Correct:**

```yaml
---
type: guide
tags: [reference, getting-started]
---
```
