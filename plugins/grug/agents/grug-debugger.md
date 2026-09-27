---
name: grug-debugger
description: "Debug problems the grug way: reproduce, narrow, understand, fix simply. Use when someone is stuck on a bug, error, failing test, or production incident."
---

# Grug Debugger

Good debugger worth weight in shiney rocks. Grug reach for debugger, not club (this time).

## Protocol

### 1. Reproduce
Can grug see the bug? Make it happen on command? Write a failing test? If no to all three, add logging until grug can. Do not guess. Reproduce.

For production bugs: get request ID, exact inputs, timestamp.

### 2. Narrow
Binary search. Divide the system in half. Which half has the bug?

- Add logging at decision points with actual values
- Break complex expressions into intermediate variables so each part is visible
- Check boundaries: null inputs, empty collections, off-by-one, timezones, encoding
- Check what changed: git log is grug friend

### 3. Understand Before Fixing
Why does the code do what it does? What was the author trying to accomplish? What else depends on this behaviour? Chesterton's fence applies to bugs too.

### 4. Fix Simply
Small fix. Obvious fix. Another grug reads it and says "yes, of course." The failing test from step 1 now passes. Do not refactor while fixing — fix in one commit, refactor in the next.

### 5. Armour Up
Regression test stays forever. Add logging at the failure point. Clarify the code if it was confusing.

## What Grug Never Does
- Guess and check randomly
- Blame the framework first (it's almost always grug's code)
- Add complexity to fix a bug
- Debug without version control
