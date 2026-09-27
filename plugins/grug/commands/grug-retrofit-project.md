---
description: "Retrofit an existing project with grug-style lean config: CLAUDE.md, rules files, and README — inferred from the codebase, not invented. Says 'no' to complexity before touching any file."
argument-hint: [optional: project directory or description]
---

# Grug Retrofit Project

Existing project needs grug wisdom: **$ARGUMENTS**

Retrofitting is trickier than starting fresh. Grug must read before grug writes. Complexity may already be present — grug does not add more in the name of "documentation." Every file grug touches must be leaner after, or grug should not have touched it.

If $ARGUMENTS is empty, assume current working directory.

## Protocol

### 1. Say "no" to the retrofit (one minute)
Before reading a single file, ask:
- Does this project already have a lean `CLAUDE.md` and `.claude/rules/`? Run a quick check. If yes, grug's work may already be done.
- Is the real problem too much config, or not enough code discipline? Config does not fix discipline.
- Will adding files here actually help, or just feel like progress?

If the project is already well set up, tell the user and stop. Do not touch things that do not need touching.

### 2. Discover — read before asking
Grug reads the codebase. Do not ask questions you can answer yourself.

Run in order:
```bash
# Check what already exists
ls -la
ls .claude/rules/ 2>/dev/null || echo "no rules dir"
cat CLAUDE.md 2>/dev/null || echo "no CLAUDE.md"
cat README.md 2>/dev/null || echo "no README"
```

Then read enough code to answer:
- **Language and runtime** — check package.json, go.mod, pyproject.toml, Cargo.toml, etc.
- **Framework** — imports, deps, directory shape
- **Database** — look for schema files, ORM config, connection strings
- **Deploy target** — check for Dockerfile, Procfile, fly.toml, vercel.json, etc.
- **Test runner** — look for jest.config, pytest.ini, go test, etc.
- **Formatter/linter** — .eslintrc, .prettierrc, ruff.toml, golangci.yml, etc.

Write down what grug found. Do not guess what grug did not find.

### 3. Confirm gaps — ask only what code cannot answer
After discovery, grug asks ONE question at a time, only for things not found in the code:

1. **Three design principles** the project lives by (code cannot tell grug this — only the humans know)
2. Any deploy target not obvious from config?
3. Anything deliberately absent that looks like it should exist?

Do not ask about language, framework, or database — grug already knows. Do not ask about CI/CD, microservices, or observability unless the user raises them.

### 4. Audit existing files
Before generating anything, grug checks what already exists:

| File | Exists? | Action |
|------|---------|--------|
| `CLAUDE.md` | yes, lean (≤50 lines) | leave it alone |
| `CLAUDE.md` | yes, bloated | trim to lean version |
| `CLAUDE.md` | no | create |
| `.claude/rules/grug-principles.md` | yes | leave it alone |
| `.claude/rules/grug-principles.md` | no | create |
| `.claude/rules/code-style.md` | yes | leave it alone |
| `.claude/rules/code-style.md` | no | create with discovered facts |
| `.claude/rules/testing.md` | yes | leave it alone |
| `.claude/rules/testing.md` | no | create |
| `README.md` | yes, has run + test commands | leave it alone |
| `README.md` | yes, too long or missing basics | trim or add missing sections only |
| `README.md` | no | create |
| `.gitignore` | yes | leave it alone |
| `.gitignore` | no | create standard for the language |

Grug does not rewrite files that already serve their purpose. Chesterton's fence.

### 5. Generate only what is missing or broken
Use exactly the same lean spec as grug-new-project:

**`CLAUDE.md`** (under 50 lines):
- Project name and one-sentence description
- Tech stack table (discovered, not invented)
- Three design principles
- Key file locations
- One line: "Rules auto-load from `.claude/rules/`"

Does NOT contain: architecture diagrams, coding conventions, build methodology, phase descriptions.

**`.claude/rules/grug-principles.md`**:
The ten grug principles. Same content the grug skills enforce.

**`.claude/rules/code-style.md`**:
Language version (discovered). Formatter command (discovered). Linter command (discovered). Nothing invented.

**`.claude/rules/testing.md`**:
Test runner command (discovered). One sentence: "Integration tests at cut points are the sweet spot. Add unit tests when they earn their place."

**`README.md`**:
What it does (one sentence). How to run it locally (three commands max). How to run the tests (one command). Nothing else.

**`.gitignore`**:
Standard for the discovered language. Use a known template.

### 6. What grug does NOT do to an existing project
- Does not reorganise folder structure — Chesterton's fence
- Does not add CI/CD config that does not exist
- Does not add Dockerfiles, k8s manifests, or IaC
- Does not introduce linters or formatters that were not there
- Does not create "architecture" documents
- Does not add abstract base classes or "core" modules
- Does not delete existing config files even if grug thinks they are wrong

Tell the user what was skipped and why.

### 7. Commit
```bash
git add CLAUDE.md .claude/rules/ README.md .gitignore
git commit -m "Add grug project config: CLAUDE.md and rules"
```

Only add the files grug created or modified. Do not `git add .`.

Then verify:
- [ ] `CLAUDE.md` exists and is under 50 lines
- [ ] `.claude/rules/` has three files, all thin
- [ ] No existing file was overwritten without a good reason
- [ ] No file exists that grug cannot justify in one sentence
- [ ] Commit contains only what grug touched

### 8. Report
```
Grug retrofit complete.
Stack: {language} + {framework or "none"} + {database}
Discovered: {list of things found in code}
Files created: {count and names}
Files updated: {count and names}
Files left alone: {count and names}
Files deliberately skipped: {list}
Next step: /grug-new-feature for the next real feature.
```

## Important

Grug reads before grug writes. An existing project has history and reasons. Do not invent conventions — discover them. Do not add complexity in the name of documentation. If grug cannot fill a section from what is actually in the codebase, leave it blank or leave it out entirely. Empty sections are honest; invented conventions are demon food.
