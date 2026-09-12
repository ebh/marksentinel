---
type: reference
title: read-first-repeated
description: A stated hard prerequisite shouldn't also appear as a later, unanchored see-also.
---

# `read-first-repeated`

**Severity:** WARN · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

If a document's `Read first:` clause names another document as a hard prerequisite, that same
document must not also turn up later in the body as a bare, unanchored link — a plain
see-also-style reference to the whole document, not to one specific section of it. A later
link that carries a `#anchor`, citing one particular section of the prerequisite, is fine and
doesn't trigger this.

## Why

Naming something as a hard prerequisite once, up front, is a clear statement. Linking the same
document again later as a plain see-also implies the relationship is optional or incidental —
which contradicts what the `Read first:` block already said.

## How to fix

Remove the later bare link (the reader already has it from the `Read first:` block), or turn
it into an anchored link to the specific section you actually mean to point at.

## Examples

**Incorrect:**

```markdown
# Getting started

> **Read first:** [installation](/docs/install.md)

...

See also [installation](/docs/install.md) for setup steps.
```

**Correct** (an anchored citation of a section is fine):

```markdown
...

See [the prerequisites section](/docs/install.md#prerequisites) for the exact versions.
```
