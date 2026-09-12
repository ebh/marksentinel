---
type: reference
title: frontmatter-type
description: A document's frontmatter must declare a non-empty `type`.
---

# `frontmatter-type`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

A document's frontmatter must include a `type` key, and its value must be non-empty — not
missing, and not just whitespace.

## Why

`type` is the one frontmatter field every non-reserved document is required to carry: it's the
profile's minimal claim about what kind of document this is, and it's the one field other
tooling can always rely on being present. A missing or blank `type` leaves nothing for any
downstream reader or reviewer to key off.

## How to fix

Add a `type` key with a real value describing what kind of document this is (for example
`guide`, `reference`, `how-to`), consistent with however your bundle categorizes documents.

## Examples

**Incorrect:**

```yaml
---
title: Getting started
---
```

```yaml
---
type: ""
title: Getting started
---
```

**Correct:**

```yaml
---
type: guide
title: Getting started
---
```
