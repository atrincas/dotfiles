---
name: no-commit
description: Hold all changes in the working tree for manual review — implement freely, but never commit, stage, or open a PR. Use when the user says no commit, don't commit, leave it uncommitted, or wants to review the diff before anything lands in git history.
---

Do the work. Leave git alone.

For the rest of this session:

- Do not run `git commit` in any form — no `--amend`, no `git -C <path> commit`, and not inside a compound command.
- Do not run `git add`. Unstaged changes keep `git diff` as the review command and keep the user's own work in progress separate from yours.
- Do not run `gh pr create` — a PR implies a commit.

When the implementation is done, do not ask for permission to commit. The turn ends there.

Only an explicit later instruction to commit releases this hold. Approving a plan does not. A passing test suite does not. An earlier "commit it when done" in the same prompt that invoked this skill does not.
