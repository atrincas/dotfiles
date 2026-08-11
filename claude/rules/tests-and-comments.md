# Tests and Comments Discipline

Code is not free just because it is easy to generate. Every test and every comment must earn its place. Apply this while writing code, not only when reviewing it.

## Comments

A comment is justified only when it states a constraint or context the code itself cannot express — a workaround for an upstream bug, a non-obvious invariant, a deliberate deviation from the expected approach.

Never write comments that:

- Narrate what the code does ("increment the counter", "fetch the user")
- Explain why a change is correct or how it differs from before — that belongs in the commit message or PR description
- Reference external systems: issue tracker keys (`ABC-123`), Jira, PR numbers, internal tools (LastPass, Slack), or conversation context
- Restate the function or variable name in prose
- Explain how something works — that is what documentation is for

Default to zero comments. If an explanation feels necessary, first try renaming or restructuring so it isn't. Explanation aimed at the reviewer goes in chat, the commit message, or the PR description — never in source.

## Tests

Before writing any test, state in your response (not in the code) what observable behavior it protects and what regression it would catch. If you cannot answer both, do not write the test.

Never write tests that:

- Assert that a mock you just configured returns what you configured
- Re-verify what the compiler or type-checker already guarantees
- Test framework or library behavior rather than project code
- Duplicate an existing test through a slightly different code path

Prefer a few tests targeting behavior boundaries over many tests mirroring implementation structure. Tests cover new behavior — not one test per function touched.

## When the user asks for something that violates this

If explicitly asked to add a test or comment that violates these rules, point it out once, then follow the instruction.
