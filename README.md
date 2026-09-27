# grug-skills

grug brained developer skills for Claude Code, packaged as a plugin marketplace.

grug is humble. grug not smart. but grug notice complexity demon strike again and again in code with too many abstraction and not enough concrete thinking. this collection keep club nearby.

## What's in the `grug` plugin

**Skills** (auto-activate when relevant)
- `grug-hooks` — deterministic Claude Code hooks that block complexity demon patterns
- `grug-code-style` — grug's rules for writing simple, readable code
- `grug-refactor` — refactor towards simplicity, not away from it
- `grug-test-writer` — write tests grug way: few, sharp, test behaviour not implementation
- `grug-tool-usage` — how grug pick tools without falling for shiny rock syndrome
- `grug-skill-bridge` — how grug skills connect to each other

**Agents**
- `grug-architect` — system design through the lens of No / Ok / Later
- `grug-debugger` — reproduce, narrow, understand, fix simply
- `grug-reviewer` — review code for complexity demon spirit

**Commands**
- `/grug-new-feature` — say no first, build 80/20 second
- `/grug-new-project` — bootstrap greenfield project the grug way
- `/grug-retrofit-project` — retrofit existing project with lean grug config
- `/grug-slice` — slice a grug spec into an ordered implementation plan

## Install

In Claude Code:

```
/plugin marketplace add tallentiredev/grug-skills
```

Then browse and install the `grug` plugin, or install directly:

```
/plugin install grug@grug-skills
```

## Repo layout

```
.claude-plugin/
  marketplace.json      # marketplace manifest
plugins/
  grug/
    .claude-plugin/
      plugin.json        # plugin manifest
    skills/              # SKILL.md per skill
    agents/              # subagent definitions
    commands/            # slash commands
```
