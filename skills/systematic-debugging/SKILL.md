---
name: systematic-debugging
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
---

# Systematic Debugging

## Overview

Random fixes waste time and create new bugs. Quick patches mask underlying issues.

**Core principle:** ALWAYS find root cause before attempting fixes. Symptom fixes are failure.

**Violating the letter of this process is violating the spirit of debugging.**

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you cannot propose fixes.

## When to Use

- Test failures, bugs, unexpected behavior, performance problems, build/integration failures
- Under time pressure or when "just one quick fix" seems obvious
- After multiple failed fix attempts or when you don't fully understand the issue
- When issue seems simple (simple bugs have root causes too)
- When rushing (systematic is faster than thrashing)

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully** -- Don't skip past errors. Read stack traces completely. Note line numbers, file paths, error codes. They often contain the exact solution.

2. **Reproduce Consistently** -- Can you trigger it reliably? What are the exact steps? If not reproducible, gather more data -- don't guess.

3. **Check Recent Changes** -- Git diff, recent commits, new dependencies, config changes, environmental differences.

4. **Gather Evidence in Multi-Component Systems** -- For systems with multiple components, add diagnostic instrumentation at every component boundary before proposing fixes. Log inputs/outputs at each layer, run once to pinpoint the failing boundary. See `multi-component-debugging.md` for detailed examples and templates.

5. **Trace Data Flow** -- See `root-cause-tracing.md` for the complete backward tracing technique. Quick version: Where does the bad value originate? Trace callers upward until you find the source. Fix at source, not at symptom.

### Phase 2: Pattern Analysis

**Find the pattern before fixing:**

1. **Find Working Examples** -- Locate similar working code in same codebase.
2. **Compare Against References** -- Read reference implementations COMPLETELY, not skimming.
3. **Identify Differences** -- List every difference between working and broken, however small.
4. **Understand Dependencies** -- Other components, settings, config, environment, assumptions.

### Phase 3: Hypothesis and Testing

**Scientific method:**

1. **Form Single Hypothesis** -- State clearly: "I think X is the root cause because Y." Be specific.
2. **Test Minimally** -- Smallest possible change. One variable at a time. Don't fix multiple things at once.
3. **Verify Before Continuing** -- Worked? Phase 4. Didn't work? Form NEW hypothesis. Don't stack fixes.
4. **When You Don't Know** -- Say "I don't understand X." Don't pretend. Ask for help. Research more.

### Phase 4: Implementation

**Fix the root cause, not the symptom:**

1. **Create Failing Test Case** -- Simplest reproduction, automated if possible. MUST have before fixing. Use `superpowers:test-driven-development` skill.

2. **Implement Single Fix** -- Address root cause. ONE change at a time. No "while I'm here" improvements. No bundled refactoring.

3. **Verify Fix** -- Test passes? No other tests broken? Issue actually resolved?

4. **If Fix Doesn't Work**
   - STOP. Count fix attempts.
   - If < 3: Return to Phase 1, re-analyze with new information
   - **If >= 3: STOP and question the architecture (step 5 below)**
   - DON'T attempt Fix #4 without architectural discussion

5. **If 3+ Fixes Failed: Question Architecture**

   **Pattern indicating architectural problem:**
   - Each fix reveals new shared state/coupling/problem in different place
   - Fixes require "massive refactoring" to implement
   - Each fix creates new symptoms elsewhere

   **STOP and question fundamentals:**
   - Is this pattern fundamentally sound?
   - Are we "sticking with it through sheer inertia"?
   - Should we refactor architecture vs. continue fixing symptoms?

   **Discuss with your human partner before attempting more fixes**

   This is NOT a failed hypothesis - this is a wrong architecture.

6. **When No Root Cause Found** -- If investigation reveals issue is truly environmental, timing-dependent, or external: document what you investigated, implement appropriate handling (retry, timeout, error message), add monitoring for future investigation. But note: 95% of "no root cause" cases are incomplete investigation.

## Red Flags - STOP and Follow Process

If you catch yourself thinking any of these, STOP and return to Phase 1:
- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- "Skip the test, I'll manually verify"
- "It's probably X, let me fix that"
- "I don't fully understand but this might work"
- Proposing solutions before tracing data flow
- **"One more fix attempt" (when already tried 2+)**
- **Each fix reveals new problem in different place**

**If 3+ fixes failed:** Question the architecture (see Phase 4.5)

## Common Rationalizations

| Excuse | Reality |
|---|---|
| "Issue is simple, don't need process" | Simple issues have root causes too. Process is fast for simple bugs. |
| "Emergency, no time for process" | Systematic debugging is FASTER than guess-and-check thrashing. |
| "Just try this first, then investigate" | First fix sets the pattern. Do it right from the start. |
| "Multiple fixes at once saves time" | Can't isolate what worked. Causes new bugs. |
| "One more fix attempt" (after 2+ failures) | 3+ failures = architectural problem. Question pattern, don't fix again. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|---|---|---|
| **1. Root Cause** | Read errors, reproduce, check changes, gather evidence | Understand WHAT and WHY |
| **2. Pattern** | Find working examples, compare | Identify differences |
| **3. Hypothesis** | Form theory, test minimally | Confirmed or new hypothesis |
| **4. Implementation** | Create test, fix, verify | Bug resolved, tests pass |

## Required References

- **REQUIRED:** `root-cause-tracing.md` -- Complete backward tracing technique
- **REQUIRED:** `defense-in-depth.md` -- Multi-layer validation after root cause found
- **REQUIRED:** `condition-based-waiting.md` -- Replace arbitrary timeouts
- **REFERENCE:** `multi-component-debugging.md` -- Detailed diagnostic instrumentation examples
- **REFERENCE:** `user-signals.md` -- Watch for these redirections from your partner

**Related skills:**
- **superpowers:test-driven-development** -- For creating failing test case (Phase 4, Step 1)
- **superpowers:verification-before-completion** -- Verify fix worked before claiming success
