---
type: reference
title: timestamp
description: A document's frontmatter `timestamp` must be a real calendar date.
---

# `timestamp`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

When frontmatter has a `timestamp` key, its value must be a real calendar date written as
`YYYY-MM-DD` — not just a value shaped like one. `2026-13-40` matches the pattern but isn't a
real date (there's no 13th month or 40th day), and fails this check too.

## Why

A `timestamp` that looks valid but silently rolls over into some other date — the way many
date libraries would coerce `2026-13-40` into a date the following year rather than reject it
— would record the wrong day without anyone noticing. Anything that sorts, filters, or
displays by `timestamp` inherits that error.

## How to fix

Correct the value to a real calendar date in `YYYY-MM-DD` form.

## Examples

**Incorrect:**

```yaml
---
timestamp: 2026-13-40
---
```

```yaml
---
timestamp: 09-12-2026
---
```

**Correct:**

```yaml
---
timestamp: 2026-09-12
---
```
