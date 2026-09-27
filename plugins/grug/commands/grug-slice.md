---
description: "Slice a grug spec into an ordered plan of small features, each sized for one /grug-new-feature invocation. Outputs to .claude/plans/."
argument-hint: [path to spec file, or paste spec]
---

# Grug Slice

User wants to slice: **$ARGUMENTS**

A spec is a shape. A plan is a sequence. This command turns one into the other.

If $ARGUMENTS is a file path, read it. If it's pasted content, use that. If empty, check for `docs/grug-spec*.md` in the repo. If nothing found, ask the user for their spec.

## Protocol

### 1. Read the spec
Identify:
- The cut points (these become the natural boundaries for slices)
- The "What Claude Code Should Build First" list (the author's initial ordering)
- The "No" list (hard scope boundary — slices must not drift into this)
- The "Later" list (not now, but the slices should leave room)

### 2. Apply grug slicing rules

**Each slice must:**
- Be one feature or one cut point, not both mixed together
- Be buildable in one `/grug-new-feature` session — if it takes more than a day, it's too big, split again
- Leave the system working after it lands — no slice depends on a future slice to compile or pass tests
- Have a clear "done" signal — what integration test proves this slice works?

**Ordering rules:**
- Infrastructure and data model first (the ground everything stands on)
- Cut points from inside out — implement the innermost dependency first, then the things that call it
- Happy path before edge cases — get the thing working, harden later
- UI last (if any) — it changes the most and depends on everything else

**Say no during slicing:**
- If a feature from the spec doesn't survive the 80/20 test when examined alone, mark it as "skip — does not earn its place yet"
- If two features are tightly coupled and can't be built independently, merge them into one slice (but flag this — coupling is a demon sign)
- If a feature is only needed for another feature in the "Later" list, move it to Later too

### 3. Write the plan

Save to `.claude/plans/grug-plan-{project-slug}.md`.

## Plan Template

```markdown
# Grug Plan: {project name}

> Sliced from: {spec filename or "pasted spec"}
> Generated: {date}
> Slices: {count}

## How to Use This Plan
Pick the next unchecked slice. Run `/grug-new-feature {slice title}`.
After it lands, come back here. Check the box. Ask: does the next slice still make sense?
Reorder, skip, or add slices as the codebase teaches you. The plan serves you, not the other way around.

## Slices

### [ ] 1. {slice title}
**What:** {one sentence}
**Cut point:** {which cut point this implements, or "none — infrastructure"}
**Done when:** {the integration test that proves it}
**Depends on:** {slice numbers, or "nothing"}

### [ ] 2. {slice title}
**What:** {one sentence}
**Cut point:** {which cut point}
**Done when:** {integration test}
**Depends on:** {slice numbers}

...

## Skipped
{features from the spec that grug said no to during slicing, with one-line reasons}

## Later (from spec)
{carried forward unchanged — these are not slices, they are reminders}

## Grug's Slicing Notes
{2-3 bullets on risks, coupling concerns, or places where the ordering might need to change once building starts}
```

### 4. Report

After saving the plan:
```
Grug plan ready: .claude/plans/grug-plan-{slug}.md
{count} slices from {spec feature count} spec features
{skipped count} features skipped (don't earn place yet)

Start: /grug-new-feature {first slice title}
```

## Important

The plan is a suggestion, not a contract. Grug has been wrong about ordering many times. The codebase will teach you things the spec could not know. When that happens, update the plan — reorder, skip, add. A plan that never changes is a plan nobody is reading.

Never auto-execute slices in sequence. The human pause between slices is where grug says "actually, that next one doesn't make sense anymore." That pause is the most valuable moment in the whole process.
