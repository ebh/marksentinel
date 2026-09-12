---
type: reference
title: h1-missing
description: Every non-reserved document needs an H1 heading.
---

# `h1-missing`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every non-reserved document must have an H1 heading — a `# Title` line (or its underlined
equivalent). A document with no H1 anywhere fails this check.

## Why

A document's H1 is its title as far as anything rendering the page is concerned, and other
checks — where the dependency block must sit, for one — are defined relative to "the first
thing after the H1." Without an H1, there's nothing for those rules to anchor against either.

## How to fix

Add a single `# Title` heading at the top of the document.

## Examples

**Incorrect:**

```markdown
Some content with no heading at all.
```

**Correct:**

```markdown
# Getting started

Some content.
```
