---
type: reference
title: filename
description: Every Markdown filename in a bundle must be lower-kebab-case.
---

# `filename`

**Severity:** ERROR · **Implemented?** See the [implementation status table](../../README.md#implementation-status).

## What this checks

Every Markdown filename must be lowercase letters and digits, with words separated by single
hyphens, ending in `.md` — for example `getting-started.md`. Uppercase letters, underscores,
doubled hyphens, and leading or trailing hyphens all fail this check. Only the filename itself
(the basename) is checked, not the directories it lives in.

## Why

A filename names a concept, not a person, a date, or free text. Consistent casing keeps a
document's path predictable from its title, and keeps any tooling that infers meaning from a
path (search, cross-linking, generated indexes) working the same way everywhere. Mixing
`snake_case`, `camelCase`, and `kebab-case` filenames side by side also makes it easy to
accidentally create two files that differ only in case.

## How to fix

Rename the file to lowercase letters and digits with single hyphens between words, then update
every link that points at its old path.

## Examples

**Incorrect:**

```
docs/Getting_Started.md
docs/gettingStarted.md
docs/getting--started.md
docs/2026-09-12-release-notes.md
```

**Correct:**

```
docs/getting-started.md
docs/release-notes.md
```
