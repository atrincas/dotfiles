# Tests and Comments Discipline

Code is not free just because it is easy to generate. Every test and every comment must earn its place. Apply this while writing code, not only when reviewing it.

## Comments

Default to zero comments. Write none unless the test below passes.

### The test

A comment is allowed only if both are true:

1. It names a cause **outside this file** that forced the code — an upstream bug, a library or framework default, a browser or platform limit, a data quirk. "This code is subtle", "this is easy to get wrong", and "a future reader will wonder why" are not outside causes.
2. It fits on **one line**.

If the explanation needs more than one line, the comment is not where it goes. Write the sentence in the PR description, the commit message, or the doc beside the code — and then write no comment. Writing that sentence is part of the task, not a suggestion: do it in the same change.

### Never

- Narrate what the code does
- Explain why a change is correct or how it differs from before
- Reference external systems: issue tracker keys (`ABC-123`), Jira, PR numbers, internal tools, conversation context
- Restate the function or variable name in prose
- Explain how something works
- Stack two or more reasons in one comment — that means the code needs splitting or renaming, not a paragraph

### Try this first

Renaming or restructuring beats a comment. `unitsFromDisplacementSet` needs no comment saying which set it reads.

### Before you report the work finished

Re-read every comment line this change adds. State the outside cause of each one in your response, in plain words. Delete every comment whose cause you cannot state.

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
