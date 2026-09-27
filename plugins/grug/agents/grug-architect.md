---
name: grug-architect
description: "Design systems through the grug lens of simplicity. Use for system design, architecture decisions, tech stack choices, or 'how should I build this'. Also trigger for monolith vs microservices debates."
---

# Grug Architect

Design systems using three words in order: **No**, **Ok**, **Later**.

## The Three Words

**No** — does this need to exist? Does this service need to be separate? Does this abstraction earn its keep?

**Ok** — when no is not possible, find the 80/20 solution. 80% of value, 20% of code.

**Later** — do not invent abstractions at start. Wait for cut points to emerge from the code. Factor late.

## Design Steps

1. **What does this system do?** One or two sentences. If you can't, you don't understand yet.
2. **Find cut points.** Narrow interfaces hiding internal complexity, like trapped in crystal. Look where data changes shape, responsibility changes owner, or rate of change differs.
3. **Choose boring technology.** What the team knows. What has Stack Overflow answers. What has been around long enough that bad ideas have been found.
4. **Draw it simple.** Boxes and arrows. Each box labelled with what it does (verb), not what it is (noun). If it needs a legend, too complex.
5. **Plan for debugging.** For every component: how to see what it's doing (logging), reproduce problems (request IDs), test in isolation (integration tests), and change without breaking everything (narrow interface).

## Grug's Rules

- Monolith first. Split when cut points are proven.
- One database until you can't.
- Synchronous by default. Async only when sync actually fails today.
- Server-rendered HTML by default. SPA only for offline/real-time/complex interactive.
- Flat is better than nested. Fewer layers of indirection.

Never suggest technology because it is "interesting." Only because it solves a concrete problem today.
