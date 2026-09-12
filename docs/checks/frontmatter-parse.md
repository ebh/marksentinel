---
type: reference
title: frontmatter-parse
description: A document's frontmatter block must be well-formed, flat YAML.
---

# `frontmatter-parse`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Covers several distinct problems inside an opened frontmatter block: the block is never closed
by a matching `---` (or `...`) before the file ends; a key isn't a plain word (it's quoted, or
a nested structure appears where a key is expected); the same key appears more than once; or a
value is a nested mapping (an object), which the profile doesn't support — a key may only hold
a scalar or a flat list of scalars. Any of these produces a `frontmatter-parse` finding naming
the specific problem.

## Why

Every other frontmatter check assumes the block underneath is valid, flat YAML. If the fence
never closes, or the structure holds something the profile doesn't support, there's no
reliable value for those checks to read — the profile restricts each key to a scalar or a flat
list precisely so that assumption always holds.

## How to fix

Depends on the specific problem: close the frontmatter block with `---` before any further
content; quote or rename a key so it's a plain word; remove a duplicate key (the later
occurrence wins, but is still flagged); or flatten a nested value into a plain scalar or list.

## Examples

**Incorrect — never closed:**

```markdown
---
type: guide
title: Getting started

# Getting started
```

**Incorrect — duplicate key:**

```yaml
---
type: guide
type: reference
---
```

**Incorrect — nested mapping:**

```yaml
---
type: guide
metadata:
  owner: team-docs
---
```

**Correct:**

```yaml
---
type: guide
title: Getting started
---
```
