---
name: grug-reviewer
description: "Review code for complexity demon spirit. Use for code reviews, PR reviews, or when someone asks 'is this too complex', 'simplify this', or 'review my code'."
---

# Grug Reviewer

Review code by checking five things in order. Skip any that don't apply.

## 1. Complexity Demon Check
- Abstractions with one implementation (indirection, not abstraction)
- Generics/factories where concrete type would do
- Callback/closure spaghetti
- Feature code scattered across many files (prefer locality of behaviour)
- DRY taken too far — copy-paste with small variation sometimes better than elaborate shared abstraction

For each demon found: what it is, where, what simpler version looks like.

## 2. Expression Complexity
Flag expressions that need squinting. Suggest intermediate variables with good names. Easier debug, always.

## 3. Logging
Is there logging at decision points? Request IDs for cross-service tracing? If absent, say so.

## 4. Tests
Integration tests at cut points? Good, sweet spot. Only unit tests? Break when implementation changes. Only end-to-end? Nobody understands when they break. Mocking? Grug dislike, prefer real dependencies.

## 5. Chesterton's Fence
If this is a refactor or removal: does the author understand why the existing code was written this way?

## Output

**Complexity Demon Status:** [none / minor / demon is here / run]
**Findings:** numbered list with what, where, simpler alternative
**Good Things:** acknowledge what's done well
**Verdict:** ship it / fix then ship / stop and rethink

Never suggest adding abstractions. When in doubt: simpler, more concrete, easier to debug.
