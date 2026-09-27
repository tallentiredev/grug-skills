---
name: grug-hooks
description: "Claude Code hooks that prevent complexity demon from entering codebase. Deterministic guardrails that fire every time — not suggestions the model can ignore. Use when setting up a new project, configuring Claude Code, or when someone asks about hooks, guardrails, or preventing bad patterns in AI-generated code."
---

# Grug Hooks

CLAUDE.md is suggestion. Hook is guarantee. Suggestion is polite request to demon. Guarantee is club.

These hooks go in `.claude/settings.json` so whole team gets same protection.

## The Hooks Grug Recommends

### 1. Complexity Gate (PreToolUse — Write|Edit|MultiEdit)

Before Claude writes or edits a file, check for demon signs. Prompt hook uses fast model to evaluate:

```json
{
  "PreToolUse": [{
    "matcher": "Write|Edit|MultiEdit",
    "hooks": [{
      "type": "prompt",
      "prompt": "Review this code change. Reject if any: 1) interface/abstract class with only one implementation, 2) generic type parameter that could be concrete, 3) more than 3 levels of nesting without early returns, 4) complex expression without intermediate variables. Respond with allow or deny. $ARGUMENTS"
    }]
  }]
}
```

### 2. New Dependency Gate (PreToolUse — Bash)

Block `npm install`, `pip install`, `cargo add` without thinking. Exit 2 = blocked.

```json
{
  "PreToolUse": [{
    "matcher": "Bash",
    "if": "Bash(npm install *|pip install *|cargo add *|yarn add *|pnpm add *|dotnet add *)",
    "hooks": [{
      "type": "prompt",
      "prompt": "A new dependency is being added. Reject unless the task genuinely cannot be done with existing dependencies or standard library. Every dependency is future upgrade burden and CVE risk. $ARGUMENTS"
    }]
  }]
}
```

### 3. File Count Check (Stop)

When Claude finishes, check if it touched too many files. Grug gets suspicious above 5.

```json
{
  "Stop": [{
    "hooks": [{
      "type": "prompt",
      "prompt": "Review the changes made in this session. If more than 5 files were created or modified for a single feature, flag this and suggest ways to simplify. Warn if new abstractions were introduced without a second user. $ARGUMENTS"
    }]
  }]
}
```

### 4. Logging Check (PostToolUse — Write|Edit|MultiEdit)

After code is written, check logging exists at decision points.

```json
{
  "PostToolUse": [{
    "matcher": "Write|Edit|MultiEdit",
    "hooks": [{
      "type": "prompt",
      "prompt": "Check this code for logging. Every major if/else branch should have a log line with actual values, not just 'entering function'. If logging is absent from decision points, give feedback suggesting where to add it. $ARGUMENTS"
    }]
  }]
}
```

### 5. Project File Gate (PreToolUse — Write|Edit|MultiEdit)

.csproj and .fsproj files are where dotnet dependencies live. Claude can sneak packages in by editing these directly, bypassing the bash dependency gate. Catch it at the door.

```json
{
  "PreToolUse": [{
    "matcher": "Write|Edit|MultiEdit",
    "if": "Write(*.csproj)|Write(*.fsproj)|Edit(*.csproj)|Edit(*.fsproj)|MultiEdit(*.csproj)|MultiEdit(*.fsproj)",
    "hooks": [{
      "type": "prompt",
      "prompt": "A .csproj or .fsproj file is being modified. If a new PackageReference is being added, reject unless genuinely needed — cannot be done with existing packages or BCL. If project properties or structure are changing, flag why. $ARGUMENTS"
    }]
  }]
}
```

### 6. Dangerous Command Block (PreToolUse — Bash)

Standard safety. Exit 2 blocks.

```json
{
  "PreToolUse": [{
    "matcher": "Bash",
    "if": "Bash(rm -rf *|DROP TABLE*|git push --force*|git reset --hard*)",
    "hooks": [{
      "type": "command",
      "command": "echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"deny\",\"permissionDecisionReason\":\"Grug say no to dangerous command. Think first, club later.\"}}'"
    }]
  }]
}
```

### 7. Session Start Context (SessionStart)

Inject grug principles at the start of every session.

```json
{
  "SessionStart": [{
    "hooks": [{
      "type": "command",
      "command": "echo 'GRUG RULES: 1) Complexity very bad. Say no first. 2) 80/20 solution. 3) Intermediate variables with good names. 4) Flat over nested. 5) Log at decision points. 6) Integration tests at cut points. 7) No new abstractions without second user. 8) Understand before changing (Chesterton fence).'"
    }]
  }]
}
```

## Full settings.json

Combine all hooks into one file. Pick what grug needs, leave the rest:

```json
{
  "hooks": {
    "SessionStart": [{"hooks": [{"type": "command", "command": "echo 'Remember: complexity very very bad. Say no first. 80/20 second. Easy debug always.'"}]}],
    "PreToolUse": [
      {"matcher": "Write|Edit|MultiEdit", "hooks": [{"type": "prompt", "prompt": "Reject if: interface with one implementation, unnecessary generics, >3 nesting levels without early returns, complex expressions without intermediate variables. $ARGUMENTS"}]},
      {"matcher": "Bash", "if": "Bash(npm install *|pip install *|cargo add *|yarn add *|pnpm add *|dotnet add *)", "hooks": [{"type": "prompt", "prompt": "Reject unless task genuinely cannot be done with existing dependencies or stdlib. $ARGUMENTS"}]},
      {"matcher": "Write|Edit|MultiEdit", "if": "Write(*.csproj)|Write(*.fsproj)|Edit(*.csproj)|Edit(*.fsproj)|MultiEdit(*.csproj)|MultiEdit(*.fsproj)", "hooks": [{"type": "prompt", "prompt": "If new PackageReference being added, reject unless genuinely needed. If project structure changing, flag why. $ARGUMENTS"}]},
      {"matcher": "Bash", "if": "Bash(rm -rf *|DROP TABLE*|git push --force*|git reset --hard*)", "hooks": [{"type": "command", "command": "echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"deny\",\"permissionDecisionReason\":\"Grug say no.\"}}'" }]}
    ],
    "PostToolUse": [
      {"matcher": "Write|Edit|MultiEdit", "hooks": [{"type": "prompt", "prompt": "Check for logging at decision points. Flag if/else branches without log lines. $ARGUMENTS"}]}
    ],
    "Stop": [
      {"hooks": [{"type": "prompt", "prompt": "If >5 files changed for one feature, suggest simplification. Flag new abstractions without second user. $ARGUMENTS"}]}
    ]
  }
}
```

## Grug's Hook Philosophy

- **Command hooks** for hard rules (block dangerous commands). Deterministic. Fast. No demon negotiation.
- **Prompt hooks** for taste judgements (complexity, logging, style). Uses fast model. Cheaper than agent hook.
- **Agent hooks** grug avoids unless truly needed. Heavy. Complex. Ironic to fight complexity with complexity.
- Start with SessionStart + one PreToolUse. Add more only when pain is real. Hooks are tools too — grug-tool-usage rules apply.
