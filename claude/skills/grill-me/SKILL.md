---
name: grill-me
description: Interview the user relentlessly about a plan or design, one question at a time, until every branch of the design tree is resolved and the terms and trade-offs are written down. Use when the user wants to stress-test a plan or design, or writes "grill me" or "/grill-me" anywhere in a message.
---

Run the mattpocock-skills grilling workflow, with one override.

## Invoke

1. `no-commit` — a grilling session ends in shared understanding, not in git history.
2. `mattpocock-skills:grilling` — the design tree, the frontier, the `❓ Q<n>` format.
3. `mattpocock-skills:domain-modeling` — `CONTEXT.md` for terms, `docs/adr/` for decisions.

2 and 3 together are what `mattpocock-skills:grill-with-docs` runs. That skill sets `disable-model-invocation: true`, so it is reachable only when the user types it; invoke the two underlying skills instead.

Follow both skills as written. In particular, `domain-modeling` owns when an ADR gets written — all three of hard to reverse, surprising without context, and a real trade-off, or no file — and `ADR-FORMAT.md` caps it at one to three sentences. Do not widen that gate.

## The override: one question per message

`grilling` says to ask the whole frontier in one round. Do not. Everything else in it stands.

- **One question per message.** Not two. Then wait.
- **One open question at a time.** Never ask a new question while an earlier one is unanswered. If an answer made an earlier question moot, withdraw it by name in the same message.
- **Never `AskUserQuestion`**, or any other interactive picker, for a grilling question. The picker truncates option bodies, so the reasoning that makes an option choosable is what gets cut. Grilling questions are markdown, in your own message. This holds in plan mode: `AskUserQuestion` is for clarifying scope before the grill starts, and `ExitPlanMode` is for plan approval.

Number `<n>` cumulatively across the session. Never reset it, never reuse a number. Recompute the frontier after every answer — an answer routinely collapses or reframes questions that looked independent.

This override has failed twice before, both times because `grilling`'s text arrives after the instruction that overrides it. If you have just read `grilling` and are about to number a second question in this message, that is the failure happening again. Send the first one and wait.
