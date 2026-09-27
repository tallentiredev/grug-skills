---
description: "Start a new feature the grug way: say no first, build 80/20 second, prevent complexity demon from entering the codebase."
argument-hint: [feature description]
---

# Grug New Feature

User wants to build: **$ARGUMENTS**

Every feature is a door for complexity demon. Grug guards the door. Walk through this protocol before writing any code.

## Protocol

### 1. Say "no" (out loud this time)
- Does this feature need to exist? What problem does it solve, for whom, how often?
- Can existing code or features handle it already?
- What is the cost of NOT building it? If "nothing bad happens," push back.

If the feature does not survive this step, tell the user and stop. Do not build.

### 2. Find the 80/20
- What is the single most important thing this feature must do?
- What can be left out and still deliver value?
- Write the **"no" list** explicitly — what this feature will NOT do. The no list protects the codebase.

### 3. Build on existing patterns
- Read the codebase first. Same file structure, data access, error handling, and logging as similar features.
- Consistency beats cleverness. Chesterton's fence — existing patterns exist for reasons.

### 4. Grug code style
- Intermediate variables with good names
- Flat over nested (early returns)
- Concrete over abstract (no interface with one implementation)
- Log at decision points
- No new dependencies unless pain is real

### 5. One integration test
- Call the public API of the feature
- Use real dependencies where practical
- Mock only external services
- Assert on outcomes, not implementation

### 6. Demon check before declaring done
- Files touched > 5? Suspicious — justify or simplify.
- New abstractions > 0? Justify each one (must have second user).
- Read the diff as a stranger. Anything clever? Remove it.

## Output

Produce in this order:
1. **Feature in one sentence**
2. **What grug said no to** (the no list)
3. **The 80/20 plan**
4. **Files to touch**
5. **Test plan**
6. **Code**

If $ARGUMENTS is empty, ask the user what feature they want to build before starting.
