---
name: test-map
description: Render a test file as a readable map — a tree of its tests, each as given/when/then, with the shared setup stated once and the regression each test catches. Use when the user says a test file is hard to read, asks what a test file covers or checks, asks to summarise, map, outline or explain tests, or points at a `.test.` or `.spec.` file and wants to know what is in it.
---

# Map a test file

Show what a test file checks, without the reader opening the file.

Only read it. Do not change the test. Do not change the code under test. Do not commit. If you find a problem, write the fix in words in the last section, and stop.

## Select the target

Use the file that the user names. If the user names a source file, use the test file for it. If the user names nothing, use the test file that the session last touched.

For a directory or a glob, print one block for each file: the path, the number of tests, and the one-line `Covers` sentence. Do not print the full map for more than three files.

## Read the source

Build the map from the source of the test file. Do not run the suite.

There is one exception. `it.each`, `test.each`, and a loop over a fixture array make the names at collection time, so the names are not in the source. Only in that case, run the collection command of the framework:

| Framework | Command |
|---|---|
| Vitest | `npx vitest list <file>` |
| Playwright | `npx playwright test <file> --list` |
| Mocha | `npx mocha <file> --dry-run --reporter spec` |
| `node --test` | `node --test --test-reporter=spec <file>` |
| Jest | none. Jest has no dry run for test names, and `--listTests` prints file paths only. Read the names from the source, and tell the user that the names come from the source. |

Collection runs the code at module level and the body of each `describe`. This can start a server or open a database. This is the second reason to use collection only for dynamic names.

Find the framework in the imports of the test file. If the imports do not show it, read `package.json`.

## Print this map

```
tests/auth/verifyToken.test.ts — vitest — 9 tests in 3 describes

Covers: verifyToken() rejects bad tokens and returns the claims of good ones.

Shared setup — applies to every test below
  clock frozen at 2024-01-01T00:00:00Z (vi.useFakeTimers)
  findUser mocked, returns { id: 'u1', revokedAt: null }
  signToken(ttl) builds a valid HS256 token

verifyToken()
  1  returns the claims of a valid token
       given  a token signed 5 minutes ago
       when   verifyToken(token)
       then   returns { sub: 'u1', role: 'admin' }
       catches  a claim that the decode step drops

  2  throws when the token expired
       given  the clock moves 2 hours forward
       when   verifyToken(token)
       then   throws TokenExpiredError
       catches  an expiry check that someone removes from the guard

  when the user is revoked
  3  ...

Read first: 1, 2. The other tests are edge cases on the same path.
```

Obey these rules for the map:

- `given` gives the state that the assertion needs, with the values resolved. Write the value that the factory or the `beforeEach` makes. Do not write the name of the helper. This is the purpose of this skill.
- State in `Shared setup` what applies to every test. State it one time. Do not repeat it in a test.
- Write one `given`/`when`/`then` group for each test. If a test needs two groups, write one group, and report the test in `Problems`.
- `catches` gives the regression that the test stops. If the test protects no behaviour, write `catches nothing`, and report the test in `Problems`.
- `Read first` gives the two or three tests that hold the primary behaviour.

## End with the problems

Report these problems. If you find none, write one sentence that says so. Do not invent a problem.

- The name of the test does not agree with the assertion.
- The test asserts a mock that the same test configures.
- Two tests have the same assertion and different values only.
- The test holds more than one behaviour.
- The setup is for every test, but a minority of the tests use it.
- The test has no assertion.

These are the failures in the `tests-and-comments` rule. Use the words of that rule.

## Write in Simplified Technical English

- Write a maximum of 25 words in a sentence, and one topic in a paragraph.
- Use the active voice and the simple tenses.
- Use the same word for the same thing every time.
- Do not use emoji, praise, or filler words.
