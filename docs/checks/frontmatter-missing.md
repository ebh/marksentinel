---
type: reference
title: frontmatter-missing
description: Every non-reserved document needs a YAML frontmatter block.
---

# `frontmatter-missing`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every non-reserved Markdown document (anything that isn't `index.md` or `log.md`) must open
with a YAML frontmatter block — a `---` fence, some `key: value` pairs, and a closing `---`
(or `...`) fence. A document with no frontmatter block at all fails this check. (`index.md`
and `log.md` follow the opposite rule — see `frontmatter-reserved` — and are exempt from this
one.)

## Why

Frontmatter is the only place a document's metadata is machine-readable. Building an index,
closing a tag vocabulary, or reasoning about a bundle as a whole all start by reading a
document's frontmatter — a document with none gives those checks nothing to work from.

## How to fix

Add a frontmatter block as the very first thing in the file:

```yaml
---
type: guide
title: Getting started
description: How to set up a local development environment.
---
```

At minimum, `type` is required (see `frontmatter-type`) — everything else is recommended.

## Examples

**Incorrect:**

```markdown
# Getting started

Some content with no frontmatter at all.
```

**Correct:**

```markdown
---
type: guide
title: Getting started
description: How to set up a local development environment.
---

# Getting started

Some content.
```
