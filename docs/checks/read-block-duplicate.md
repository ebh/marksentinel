---
type: reference
title: read-block-duplicate
description: A document may declare at most one dependency block.
---

# `read-block-duplicate`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

A document may declare at most one dependency block — the `**Read first:**` / `**Read with:**`
blockquote stating what a reader needs first. If a second, separate blockquote elsewhere in the
document also opens with that same pattern, that's a duplicate and fails this check. A single
blockquote that combines both a `Read first:` clause and a `Read with:` clause still counts as
one block, not two — see [`read-block-position`](read-block-position.md) for where that one
block has to sit.

## Why

A document's dependency chain is meant to be a single, unambiguous statement. Two separate
blocks — especially if they say different things — leave a reader unsure which one to trust.

## How to fix

Merge the two blockquotes into one (combining both clauses if needed), and remove the extra
block.

## Examples

**Incorrect:**

```markdown
# Getting started

> **Read first:** [installation](/docs/install.md)

Some content.

> **Read with:** [configuration](/docs/config.md)
```

**Correct:**

```markdown
# Getting started

> **Read first:** [installation](/docs/install.md)
> **Read with:** [configuration](/docs/config.md)

Some content.
```
