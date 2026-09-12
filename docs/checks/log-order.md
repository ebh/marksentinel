---
type: reference
title: log-order
description: A log.md's date headings must appear in descending order.
---

# `log-order`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Only applies to `log.md`. The date headings in a log must appear in descending order — each
one no later than the one above it. A heading whose date is chronologically after the one
immediately preceding it fails this check.

## Why

A log reads newest-first, by convention — that's the point of a changelog-style document. An
out-of-order date breaks that convention and makes the log harder to scan.

## How to fix

Move the misplaced entry so dates descend from top to bottom.

## Examples

**Incorrect:**

```markdown
# Log

## 2026-09-01

Earlier entry.

## 2026-09-12

Later entry, placed below the earlier one.
```

**Correct:**

```markdown
# Log

## 2026-09-12

Later entry.

## 2026-09-01

Earlier entry.
```
