---
type: reference
title: log-heading
description: Every heading in a log.md must be a real calendar date.
---

# `log-heading`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Only applies to `log.md`. Every `##`-level heading in a log must be a date in `YYYY-MM-DD`
form, using the same real-calendar-date rule as [`timestamp`](timestamp.md). A `##` heading
that isn't a valid date fails this check.

## Why

A log is a chronological record — every entry is keyed to a real date. Checking only the shape
of a heading, without confirming it's an actual date, would let something like
`## 2026-13-40` or `## Overview` through as if it were a normal dated entry.

## How to fix

Rewrite the heading as a real calendar date, or remove it if it isn't meant to be a dated
entry — `log.md`'s `##` headings are reserved for dated entries.

## Examples

**Incorrect:**

```markdown
# Log

## Overview

## 2026-09-12

Some entry.
```

**Correct:**

```markdown
# Log

## 2026-09-12

Some entry.
```
