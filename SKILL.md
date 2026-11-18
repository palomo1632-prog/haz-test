---
name: haz-test
description: Write or complete a project's tests with the "expect tests" philosophy — assert the expected visible outcome, run the real code, stub only external edges. First detects whether the project already tests this way and follows it; if not, it implements it. Use when the user says "haz test", "write tests", "add tests", "tests for this", "regression test for this bug", or invokes /haz-test.
---

# Haz test

Goal: tests that declare the expected visible outcome ("given X, expect Y") and run the real
code. A test must fail when a business rule breaks — not when someone refactors.

## Step 0 — Read the project first (always)

Find out by reading the repo; never ask what you can see:

- Is there a test runner and a test command? (`package.json`, `pytest`, `go test`, `phpunit`,
  `cargo test`…)
- Where do tests live and how are they named?
- Do tests call the real code and stub only external edges, or do they mock everything?
- Do they assert visible outcomes and public contracts, or reimplement the logic?
- Is there a testing-convention doc?

Decision:

- **Already uses this style** → follow its conventions (runner, folders, names, helpers,
  language). Never delete or rewrite passing tests. Record the baseline (what passes and what
  fails today) and add tests only where critical coverage is missing or in the areas you touch.
- **Not this style / no tests** → implement it with the minimum: add the stack's standard runner
  without changing behavior, create the tests folder, auto-discover test files, and start with
  the critical path.

## Step 1 — Invariants

Before writing tests, list the invariants: rules that must never break (money, permissions,
user data, ordering, states). Every invariant gets at least one test.

## Rules

1. "Given X, expect Y" on what the user sees or on the public contract. Run the real code;
   never copy the logic into the test. If the test starts duplicating a calculation, replace it
   with an assert on the output.
2. Stub only external edges: network, third-party APIs, messaging, clock, production database.
   Everything else runs for real. Isolation: a temporary database or file per test.
3. One test = one rule, named in a sentence. If it comes from a bug, cite the case or ticket.
4. Fixtures use values different from production, so any hardcoded value makes the test fail.
5. Every bug fix ships a regression test that is RED without the fix and GREEN with it. Show
   both runs before closing.
6. Repeated bug: 2nd time, same test with a different stimulus; 3rd time, a matrix over the
   class of cases instead of another patch.
7. Exact-text asserts only for text the end user receives and that is approved. Never test AI
   tone or response quality: validate that with real users.
8. The suite runs with no network, deterministically and fast; auto-discover test files so none
   is left out; a single command runs everything.
9. Do not test the trivial (getters, config, static HTML) or anything that cannot cost money or
   trust. If the project has no business rules (landing page, marketing), say so and do not
   force tests: 0 to 2 is enough.

## New project

Work vertically: one invariant → failing test (red) → implementation → green → next. Never
write all tests before the code.

## Existing project

Do not touch what passes. If there is no runner, add one without changing behavior. Prioritize
money, permissions and user data; the rest gets covered in the areas you touch.

## Report at the end

- Detected style (existing or implemented) and the evidence.
- Invariants covered and which ones have no test, with the reason.
- Evidence of red → green for the regression tests.
