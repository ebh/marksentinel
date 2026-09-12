---
type: reference
title: link-anchor
description: An anchored link must resolve to a heading that actually exists.
---

# `link-anchor`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

When a link carries a `#anchor` — either a bare `#some-heading` pointing within the same
document, or `/docs/other.md#some-heading` pointing at a heading in another document — that
anchor must match an actual heading in the target document, using the same slug algorithm
GitHub uses to turn heading text into anchors. (An anchor pointing outside every discovered
documentation bundle — into source code, say — isn't checked; verifying it isn't this tool's
job.)

## Why

An anchor that doesn't match any real heading is a link that renders fine but goes nowhere
useful once clicked — the same problem as a dead link, just harder to notice, since the target
file itself does exist.

## How to fix

Fix the anchor to match the target heading's actual generated slug (lowercase, spaces turned
to hyphens, punctuation stripped) — or fix the heading, if the anchor was right and the
heading text changed since.

## Examples

**Incorrect** (actual heading is `## Setup`):

```markdown
[see the setup section](#setup-instructions)
```

**Correct:**

```markdown
[see the setup section](#setup)
```
