---
name: grug-code-style
description: "Enforce grug code style: intermediate variables, flat over nested, concrete over abstract, logging at decision points. Use for style reviews, readability improvements, or 'make this more readable'."
---

# Grug Code Style

Every rule serves one purpose: make code easier to debug at 2am when production is on fire.

## The Rules

**1. Intermediate variables with good names.** Break complex expressions apart. Each step visible in debugger. Good name does thinking for grug.

**2. Flat over nested.** Early returns. Guard clauses at top, happy path at bottom. Each level of indentation is a level of mental stack.

**3. Concrete over abstract.** Use the actual type. If only one implementation of an interface, it's indirection, not abstraction. Add abstraction when second use appears.

**4. Locality of behaviour.** Put code on the thing that does the thing. Seven files to understand one button is demon's work.

**5. Log at decision points.** Every if/else that matters gets a log line with actual values. Include IDs for tracing. Structured key-value pairs.

**6. Small functions with verb names.** `processPayment` not `paymentProcessor`. Short enough to fit one screen. But don't extract just for length — only when extracted piece has clear name and purpose.

**7. Boring names.** Variables: what it is. Functions: what it does. Booleans: is/has/can. No clever names, no abbreviations that save four characters and cost ten minutes of confusion.

## When Applying

Don't change behaviour. Run tests after each change. Apply one rule at a time. If code was already readable, leave it alone. Grug style is medicine, not vitamin.

If team has existing conventions that conflict, follow the team. Consistency within codebase beats any style guide.
