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

- An answer that introduces or redefines a domain term → add or amend the entry in `docs/domain/glossary.md`.
- An answer that closes a branch with a trade-off, an option rejected for a stated reason → write `docs/domain/adr/NNN-<slug>.md`.

Plan mode permits no writes except the plan file, so hold the entries there and write them as soon as plan mode exits.

If `docs/` is gitignored in this repo, say so and ask where the committed home should be rather than writing an untracked trail.
