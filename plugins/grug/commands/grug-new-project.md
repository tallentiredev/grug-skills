---
description: "Bootstrap a new greenfield project the grug way: minimal CLAUDE.md, lean rules files, no premature architecture. Says 'no' to complexity before any code is written."
argument-hint: [project description]
---

# Grug New Project

User wants to start: **$ARGUMENTS**

A new project is the most dangerous moment in software life. Everything is abstract, everything is possible, complexity demon spirit hovers waiting for invitation. Grug guards the door before any file is written.

If $ARGUMENTS is empty, ask what they're building (one sentence) before continuing.

## Protocol

### 1. Say "no" to the project (one minute)
Before any setup, ask:
- Does this need to be a new project, or can an existing repo absorb it?
- Is there an off-the-shelf tool that already does 80% of this?
- What is the smallest version that proves the idea?

If the project survives, continue. If not, tell the user and stop.

### 2. Interview — keep it short
Ask one question at a time. Stop when grug has enough to start.
1. **What does it do?** One sentence. If user can't, the project isn't ready.
2. **Who uses it?** One sentence.
3. **Primary language and runtime?** Pick one.
4. **One framework or none?** Boring beats exciting. No framework is also a valid answer.
5. **Database?** SQLite until proven otherwise.
6. **How is it deployed?** One target. Not three.
7. **Three design principles** the team will live by.

Do not ask about microservices, message queues, caches, observability stacks, IaC, or CI/CD platforms yet. None of those are needed on day one. If the user volunteers them, gently push back — they can be added when pain is real.

### 3. Generate the lean files

**`CLAUDE.md`** (under 50 lines, not 100):
- Project name and one-sentence description
- Tech stack table (language, framework, database, deploy target)
- Three design principles
- Key file locations (once they exist)
- One line: "Rules auto-load from `.claude/rules/`"

Does NOT contain: architecture diagrams, coding conventions, build methodology, phase descriptions, or anything that belongs in a rules file.

**`.claude/rules/grug-principles.md`**:
The ten grug principles (complexity bad, say no, 80/20, factor late, cut points, easy debug, integration tests at sweet spot, log everything, no FOLD, Chesterton's fence). Same content the grug skills enforce — having it in rules makes it auto-load every session.

**`.claude/rules/code-style.md`** (start thin):
Just the language version, formatter command, and linter command. Add real conventions only when patterns emerge from actual code. Empty sections are honest; invented conventions are demon food.

**`.claude/rules/testing.md`** (start thin):
Test runner command. One sentence: "Integration tests at cut points are the sweet spot. Add unit tests when they earn their place." That's it.

**`README.md`**:
What it does (one sentence). How to run it locally (three commands max). How to run the tests (one command). Nothing else yet.

**`.gitignore`**:
Standard for the chosen language. Use a known template, don't invent.

### 4. What grug does NOT generate
Be explicit about what's deliberately missing:
- No `architecture.html` — there is no architecture yet, only intent
- No CI/CD config — add when there is something worth running on every push
- No Dockerfile — add when deployment actually needs it
- No folder structure beyond what the language requires — let cut points emerge
- No abstract base classes, interfaces, or "core" modules
- No config system more complex than environment variables

Tell the user what was skipped and why. They can ask for any of it, but they should ask deliberately, not by default.

### 5. Initialise and verify
```bash
git init
git add .
git commit -m "Initial commit: grug project skeleton"
```

Then check:
- [ ] `CLAUDE.md` exists and is under 50 lines
- [ ] `.claude/rules/` has three files, all thin
- [ ] `README.md` explains how to run the thing
- [ ] No file exists that grug cannot justify in one sentence
- [ ] Git initialised with one clean commit

### 6. Report
```
Grug project ready.
Stack: {language} + {framework or "none"} + {database}
Files created: {count}
Files deliberately skipped: {list}
Next step: write the smallest thing that runs. Then /grug-new-feature for the first real feature.
```

## Important

Resist every urge to scaffold "best practice" structure. Best practice for an empty project is an empty project. The shape will emerge from the first three features — invent it now and grug will be wrong and stuck with the wrongness.

If the user pushes for more elaborate setup, say "ok" and build the 80/20 version. Easier to add later than remove. Removing feels like loss; adding feels like progress.
