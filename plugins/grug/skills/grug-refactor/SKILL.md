---
name: grug-refactor
description: "Perform small, safe refactors. Never too far from shore. Use when someone wants to refactor, clean up, or restructure code. Especially important when someone proposes a large refactor."
---

# Grug Refactor

System works the entire time. After every change: compiles, tests pass, behaviour unchanged.

## Before Touching Anything

**Chesterton's fence.** Read git blame, tests, comments, commit messages. If you can't explain why the code is this way, you're not ready to change it.

**Check tests exist.** No tests? Write tests for existing behaviour first. Non-negotiable.

## Protocol

1. **Name the goal.** One sentence. "Extract X so it can be tested." Not "make it cleaner" (what does that mean?) or "apply strategy pattern" (pattern is not a goal).

2. **Plan small steps.** Each step: one atomic change, keeps system working, can be committed and reverted independently. Safe steps: rename, extract function, inline unnecessary abstraction, flatten nesting, introduce intermediate variable.

3. **Execute one at a time.** Make change → run tests → commit. Do not batch. Do not "while I'm here" on unrelated code.

4. **Stop when done.** Goal achieved? Stop. "Just also clean up this other thing" is a new refactor.

## Never During Refactor
- Add new abstractions (reduce or maintain, never increase)
- Change behaviour (if assertions change, it's not a refactor)
- Introduce new dependencies
- Mix bug fixes with refactoring

## Danger Signs
- More than a day without green tests — revert, take smaller steps
- Changing files you didn't plan to — stop, reassess
- More code than the original — maybe the original was right
