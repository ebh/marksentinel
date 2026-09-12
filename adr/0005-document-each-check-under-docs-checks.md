---
status: accepted
date: 2026-09-12
deciders: [ebh]
---

# Document each check under `docs/checks/` as a focused reference page

## Context and Problem Statement

Every check marksentinel runs needs a public place a reader can go from a failing finding to
understand what fired, why the rule exists, and how to fix it. Nothing today serves that:
README's Rules section is a scoped-down summary, not written per check, and the CLI's own
output (a "point at the remediation path" line) has nowhere concrete to point.

## Decision Drivers

* Needs to be public and committed — a remediation destination that only exists internally is
  no destination at all.
* Content should be written for the person whose document just failed a check — not for
  whoever implements the check itself, which is a different job with different documentation
  needs.
* Should be writable now, per check, independent of whether that check is implemented yet —
  README's table already names every planned check, so nothing is held back by writing a
  page before the code exists.
* Whatever tracks "is this check implemented yet" should live in exactly one place, so a page
  never needs editing purely because its check shipped.

## Considered Options

* One combined `docs/checks.md` page covering every check. Rejected — doesn't scale to a
  full explanation per check across twenty-plus rules, and can't be linked to individually
  from a specific finding.
* One `docs/checks/<check>.md` page per check, with a fixed four-section shape (what it
  checks, why, how to fix, examples) and an implementation-status marker embedded in each
  page. Considered, but rejected the embedded status marker specifically (see below).
* One `docs/checks/<check>.md` page per check, same four-section shape, with implementation
  status tracked only in README's status table and linked from there — no status assertion on
  the page itself. Chosen.

## Decision Outcome

Chosen option: one `docs/checks/<check>.md` page per check — exactly four sections (what it
checks, why, how to fix, examples), nothing else. Each page carries a one-line, never-stale
pointer under its severity line to README's implementation status table, rather than
asserting its own implemented/not-implemented state. README's table links each check name to
its page once that page exists.

* A page is writable the moment a check is designed, regardless of whether it's built yet —
  nothing here waits on implementation order.
* Implementation status lives in exactly one place (README's table); a page never goes stale
  as its check ships, because it never claimed a status to begin with.
* The fixed four-section shape keeps every page skimmable by someone who just wants to fix
  their document, not read about how the check is built — implementation precision has no
  section here and stays out of scope for this doc set.
* Once every check has a page, `docs/checks/` is itself an OKF-conformant bundle marksentinel
  can eventually check against its own profile.

### Consequences

* Good, because the CLI's remediation-path output has a real, public destination to point at,
  for every check, from day one.
* Good, because a page's only maintenance cost is going stale on content (what/why/fix
  changing), never on implementation status — that risk is designed out.
* Bad, because a reader landing on a check's page directly (via search, a bookmark) has to
  follow one more link to learn whether it fires yet, rather than seeing it on the page.
* Bad, because README's table and `docs/checks/` must be kept in sync by hand — a new page
  needs its table row linked at the same time, or the two drift.
