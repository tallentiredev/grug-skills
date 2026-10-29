---
description: "Run a grug plan slice by slice with test and review gates, committing each slice to a branch, until a human decision is needed. Reads .claude/plans/."
argument-hint: [plan path] [answer to a blocked slice]
---

# Grug Run Plan

User wants to run: **$ARGUMENTS**

A plan is a sequence. This command walks the sequence so the human does not have to push it. Grug does the slices. Grug runs the tests. Grug has the reviewer look. Grug stops only when there is something a human must decide.

The pause between slices is still the most valuable moment in the process. This command keeps that pause but makes it conditional: after every slice, grug asks "does the next slice still make sense?" and stops when the answer is no.

## Roles

- **Controller** (this session): owns the gates, the commits, the plan file, and every stop decision. Keeps its own context small.
- **Implementer**: a fresh `general-purpose` subagent per slice. Builds the slice. Never judges its own work.
- **Reviewer**: the `grug-reviewer` agent. Review only, no edits.
- **Fixer**: a fresh `general-purpose` subagent given one failing gate. Same rules as the implementer.

The controller runs the gates itself and reads the output itself. A subagent saying "tests pass" is a claim, not evidence.

## Protocol

### 0. Find the plan and read the arguments

- If $ARGUMENTS has a first word ending in `.md`, that is the plan path. Everything after it is the human's **answer** to the question on the blocked slice.
- Otherwise the whole of $ARGUMENTS is the answer, and the plan is the single `.claude/plans/grug-plan-*.md`. If there are several, ask which one. If there are none, tell the user to run `/grug-slice` first and stop.
- Read the plan. Note the slice list, the "Skipped" and "Later" sections, and the spec file named in "Sliced from". Read the spec's "No" list; every implementer gets it.

### 1. Start-up checks (stop on any failure)

1. **Tree state.** Run `git status --porcelain`.
   - If the plan has a slice marked `[!]`, a dirty tree is the partial work from that stop. Allowed.
   - Otherwise the tree must be clean. If dirty, stop and ask the user what to do with the changes.
2. **Gate commands.** Find the test command and the lint command:
   - the plan header line `> Gates: test: ..., lint: ...` if present
   - else `CLAUDE.md`, then `.claude/rules/`
   - else `package.json` scripts, `Makefile`, `pyproject.toml`, `go.mod`
   If still unknown, ask the user once. Write the answer into the plan header as `> Gates: test: `...`, lint: `...``. A repo with genuinely no linter gets `lint: none`.
3. **Branch.** Use `grug-run/{plan-slug}`. If it exists, switch to it. If not, create it from the current HEAD. If switching fails, stop. Never commit to the branch the command was started from.

### 2. Pick the slice

The first slice that is `[ ]` or `[!]` and whose `Depends on` slices are all `[x]`. If none is left, go to step 4 (finish). If `[ ]` slices remain but none has its dependencies met, stop and say which dependency is missing.

### 3. Per-slice loop

#### 3a. Implement

Dispatch the implementer with this prompt, filled in:

```
You are building ONE slice of a plan. Do not build anything else.

Slice: {slice number and title}
What: {What line}
Done when: {Done when line}
Cut point: {Cut point line}
Spec No list (hard boundary): {the spec's No list}
Human answer to an earlier blocked question: {answer, or "none"}
Working tree: {"clean" or "holds partial work from an earlier attempt at this slice; continue from it"}
Earlier slices' results that matter: {Result lines of slices this one depends on}

Invoke the `grug:grug-new-feature` skill with the slice text and follow it.
Rules for this run:
- Do not commit. Do not stage. Leave changes in the working tree.
- Do not ask "want me to commit?". Do not ask the user anything. If you need
  a human decision, return BLOCKED with the question.
- If the feature fails the "say no" step, return BLOCKED and say why.
- If the slice needs a step only a human can do (run an interactive session,
  supply credentials, use an external account), return BLOCKED and say exactly
  what the human should do.
- Do not guess past the spec. A decision the spec does not cover is BLOCKED.

If you cannot invoke that skill, STOP IMMEDIATELY and RETURN BLOCKED.

Return exactly:
status: DONE | BLOCKED
files touched: {list}
new abstractions: {each, with its second user, or "none"}
test file: {path}
decisions made: {list}
deviations from the slice text: {list, or "none"}
question: {only if BLOCKED}
```

If status is BLOCKED, go to **Stop**.

#### 3b. Test gate

The controller runs, in this order, and reads the output of each:
1. the slice's "Done when" test
2. the full test suite
3. lint (skip if `lint: none`)

All three must pass. On failure, dispatch the fixer with the failing command and the tail of its output. Then re-run all three. At most **2** fix rounds, then go to **Stop**.

#### 3c. Demon check

Count files touched, excluding tests, lockfiles, docs and the plan. Also list new abstractions from the implementer's report.

- More than 5 files, or any new abstraction with no second user: go to **Stop**, and show the implementer's justification so the human can say "fine" or "simplify".

#### 3d. Review gate

Dispatch `grug-reviewer` on the slice's uncommitted diff (`git diff HEAD` plus any new files). Also ask it: does the "Done when" test assert on outcomes, or on mocks and implementation details?

- Verdict "ship it": continue.
- Verdict "fix then ship" with findings: dispatch the fixer with the blocking findings, then re-run 3b and 3c, then review again. At most **2** review rounds, then go to **Stop**.
- Verdict "stop and rethink": go to **Stop** with the reviewer's findings.
- Findings that are not blocking are kept for the final report. Do not drop them.

#### 3e. Commit and tick

1. In the plan, change `### [ ] N.` or `### [!] N.` to `### [x] N.`, remove any `**Blocked:**` line, and add under the slice:
   `**Result:** {one line}. Deviations: {list or "none"}.`
2. The tree was clean before this slice (or held only this slice's partial work), so everything in `git status --porcelain` belongs to the slice. Stage it and commit, plan included, so each slice is one commit:
   `{slice title} (slice {N})`

#### 3f. Milestone review

Count the slices committed in this run. After every 5th, run `grug-reviewer` on the whole run-branch diff since the previous milestone review (or since the branch started). Blocking findings go to the fixer and then through 3b again, as one more commit, `Review fixes after slice {N}`. Non-blocking findings go in the final report.

#### 3g. Plan check

Compare this slice's **decisions** and **deviations** against every later `[ ]` slice's What, Done when, and Depends on.

- If a later slice is now contradicted, redundant, or needs rewording, go to **Stop** with the proposed plan edit.
- Otherwise go back to step 2.

### Stop

Do all of these:
1. Mark the slice `### [!] N. {title}` in the plan, with a line under it:
   `**Blocked:** {reason}. Question: {the question for the human}`
   For a plan-check stop (no single slice is at fault), put the proposed edit under "Grug's Slicing Notes" and mark nothing.
2. Leave any uncommitted work in the working tree. Do not stash, reset or clean.
3. Do not commit the `[!]` marker. The plan file change stays uncommitted with the partial work.
4. End the run with:

```
Grug stopped on slice {N}: {title}
Why: {reason}
Question: {question}
Working tree: {clean | holds partial work: list files}
To resume: /grug-run-plan {plan path} {your answer}
```

### 4. Finish

When no `[ ]` or `[!]` slices remain, run one last `grug-reviewer` pass on the whole run-branch diff since the last milestone review. Fix blocking findings as in 3f. Then report:

```
Grug run complete: {plan path}
Branch: grug-run/{slug}
Committed: {sha} slice {N} {title}, one per line
Blocked or skipped: {list, or "none"}
Non-blocking review findings: {list, or "none"}
Not done: merge, push, PR. That is yours.
```

## What grug does NOT do

- Run slices in parallel
- Commit to the branch the run started from
- Merge, push, or open a PR
- Retry a gate more than twice. A loop that keeps trying is a loop hiding a problem
- Trust a subagent's "tests pass" without running them
- Guess past the spec. A missing decision is a question, not a coin flip
- Hide a stop. If it stopped, say so and say why

## Important

The plan is a suggestion, not a contract. The codebase teaches things the spec could not know. Each stop is the plan asking to be updated: reword the slice, skip it, or answer the question, then run the command again. Grug stopping often on the first real run is fine. Tune the stops after watching a run, not before.
