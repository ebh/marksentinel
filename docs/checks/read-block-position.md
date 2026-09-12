---
type: reference
title: read-block-position
description: A document's dependency block must be the first thing under its H1.
---

# `read-block-position`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

A document's dependency block (the `**Read first:**` / `**Read with:**` blockquote), if
present, must be the very first thing after the H1 — only blank lines are allowed between
them. A dependency block placed further down the document, after other content, fails this
check. See [`read-block-duplicate`](read-block-duplicate.md) for the related rule that there
can only be one such block at all.

## Why

The point of stating "read this first" is that a reader sees it before anything else. Burying
it after other content defeats the purpose — a reader, or an agent, that stops at the first
paragraph could miss it entirely.

## How to fix

Move the blockquote up so it's the first content directly under the H1.

## Examples

**Incorrect:**

```markdown
# Getting started

Some intro paragraph.

> **Read first:** [installation](/docs/install.md)
```

**Correct:**

```markdown
# Getting started

> **Read first:** [installation](/docs/install.md)

Some intro paragraph.
```
