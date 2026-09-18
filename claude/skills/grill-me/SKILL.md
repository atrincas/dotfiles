---
name: grill-me
description: Interview the user relentlessly about a plan or design, one question at a time, until every branch of the design tree is resolved and the terms and trade-offs are written down. Use when the user wants to stress-test a plan or design, or writes "grill me" or "/grill-me" anywhere in a message.
---

Invoke the `no-commit` skill before the first question. A grilling session ends in shared understanding, not in git history.

Do not invoke `mattpocock-skills:grilling`. Its instruction to ask the whole frontier in one round is the behaviour this skill replaces.

## The design tree

Interview the user until you reach shared understanding. Map the plan as a design tree: every decision branches into the decisions that hang off it.

The frontier is every decision whose prerequisites are already settled. Ask from the frontier, and recompute it after every answer — an answer routinely collapses or reframes questions that looked independent. A question whose answer depends on a question still open is not on the frontier. It waits.

Finding facts is your job, never the user's. When a question needs a fact from the environment, look it up or dispatch a sub-agent. Do not block on it — ask a frontier question that does not depend on the running lookup. The decisions are the user's.

The session is done when the frontier is empty. Do not act on the plan until the user confirms you have reached shared understanding.

## One question at a time

- **One question per message.** Not two.
- **One open question at a time.** Never ask a new question while an earlier one is unanswered. If an answer made an earlier question moot, withdraw it by name in the same message.
- **Never `AskUserQuestion`**, or any other interactive picker, for a grilling question. The picker truncates option bodies, so the reasoning that makes an option choosable is what gets cut. Grilling questions are markdown, in your own message. This holds in plan mode: `AskUserQuestion` is for clarifying scope before the grill starts, and `ExitPlanMode` is for plan approval.

## Format

```
❓ **Q<n>** — **<question title>**

<the decision, what hangs off it, and labeled options **A**, **B**, **C**…>

➡️ **<your pick>** (recommended) — <why, and what would change your mind>
```

Number `<n>` cumulatively across the session. Never reset it, never reuse a number.

Say what would change your mind, so the recommendation is falsifiable rather than decorative. If a decision has no sensible option set, ask it open rather than inventing options to fill the format.

## Write decisions down

Write to `docs/domain/` in the repo being worked on, as each decision settles — not when the frontier empties, so an interrupted session still leaves a record.

An answer that introduces or redefines a domain term → add or amend the entry in `docs/domain/glossary.md`. Glossary entries are cheap: write them freely.

### An ADR is the exception, not the output

Most grilling sessions write no ADR at all. Zero is the expected number. Every answer you get closes a branch and rejects an option — that is what the question format asks for — so "there was a trade-off" selects nothing.

Write `docs/domain/adr/NNN-<slug>.md` only when all three hold:

1. **Hard to reverse.** Reversing it means changing code in more than one place, or code that does not exist yet. If the next ticket can reverse it inside one file, it is not an ADR.
2. **Not visible in the code it governs.** If a reader of the function learns the decision by reading the function, the file adds nothing. The record exists for the constraint the code cannot state — a contract with another system, a limit the repo does not contain, a rule the next feature must also obey.
3. **A rejected alternative that a future reader would otherwise re-propose.** Not "we picked A over B". A rejection whose reason is still true a year later.

Two of three is no file.

### Not ADRs

- Deduplicating two components, or two copies of one function.
- Choosing which of two existing data sources a component reads.
- A deviation from a project rule with the conformant fix already scheduled. That is a ticket.
- Restating what a function does now, after a bug fix.
- Anything that would supersede an ADR written in the same feature area within the last month. That earlier ADR failed condition 1. Amend or delete it rather than stacking a second file on it.

### Size

A paragraph is a complete ADR: what was decided, and the one fact that forced it. `Considered options` and `Consequence` are optional sections. Write `Considered options` only for a rejection that would otherwise be re-proposed, and `Consequence` only for an effect a reader cannot reach from the code.

### The decisions that do not earn a file

They are not lost and they are not written to `docs/`. They go in the PR description or the commit message for the change they govern, which is where `rules/tests-and-comments.md` already sends an explanation too long to be a comment. Say in the session which decisions you routed there, so the user knows the reasoning was kept.

Plan mode permits no writes except the plan file, so hold the entries there and write them as soon as plan mode exits.

If `docs/` is gitignored in this repo, say so and ask where the committed home should be rather than writing an untracked trail.
