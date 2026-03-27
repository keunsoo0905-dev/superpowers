---
name: test-driven-development
description: Use when implementing any feature or bugfix, before writing implementation code
---

# Test-Driven Development (TDD)

## Overview

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

**Violating the letter of the rules is violating the spirit of the rules.**

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? Delete it. Start over.
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it — delete means delete

## When to Use

**Always:** New features, bug fixes, refactoring, behavior changes.

**Exceptions (ask your human partner):** Throwaway prototypes, generated code, configuration files.

Thinking "skip TDD just this once"? That's rationalization.

## Red-Green-Refactor

### RED - Write Failing Test

Write one minimal test for one behavior. Requirements:
- One behavior per test
- Clear name describing expected behavior
- Real code (no mocks unless unavoidable)

Run the test. Confirm it **fails** (not errors) for the expected reason (feature missing, not typo). Test passes? You're testing existing behavior — fix the test.

### GREEN - Minimal Code

Write the simplest code to pass the test. Nothing more.
- Don't add features beyond the test
- Don't refactor other code
- Don't "improve" beyond what the test requires

Run the test. Confirm it passes. Confirm other tests still pass. Output pristine (no errors, warnings). Test fails? Fix code, not test.

### REFACTOR - Clean Up

After green only: remove duplication, improve names, extract helpers. Keep tests green. Don't add behavior.

### Repeat

Next failing test for next behavior.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
| "Deleting X hours is wasteful" | Sunk cost fallacy. Keeping unverified code is technical debt. |
| "Keep as reference" | You'll adapt it. That's testing after. Delete means delete. |
| "TDD will slow me down" | TDD faster than debugging. Pragmatic = test-first. |

## Red Flags - STOP and Start Over

- Code before test
- Test after implementation
- Test passes immediately
- Can't explain why test failed
- Tests added "later"
- Rationalizing "just this once"
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "Keep as reference" or "adapt existing code"
- "Already spent X hours, deleting is wasteful"
- "TDD is dogmatic, I'm being pragmatic"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**

## Debugging Integration

Bug found? Write failing test reproducing it. Follow TDD cycle. Never fix bugs without a test.

## Verification

Before marking work complete: every function has a test, each test failed before implementing, minimal code written, all tests pass, output pristine, real code used, edge cases covered.

Can't verify all? You skipped TDD. Start over.

## Required References

- **REQUIRED:** `testing-anti-patterns.md` — Patterns that undermine test value
- **REFERENCE:** `tdd-detailed-guide.md` — Extended examples and full rationalization counters

## Final Rule

```
Production code -> test exists and failed first
Otherwise -> not TDD
```

No exceptions without your human partner's permission.
