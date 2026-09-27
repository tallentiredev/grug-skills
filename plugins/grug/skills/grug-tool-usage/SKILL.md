---
name: grug-tool-usage
description: "Rules for how grug uses tools, libraries, frameworks, APIs, and external services. Use whenever adopting a new tool, evaluating dependencies, choosing between libraries, or integrating external services. Also trigger for 'which library should I use', 'should I add this dependency', or 'what framework'."
---

# Grug Tool Usage

Grug love tool. Tool separate grug from dinosaurs. But tool can carry complexity demon inside like trojan horse.

## Choosing

**Before adding:** can grug solve this with what grug already has? Standard library, existing dependency, or ten lines of code? Every new dependency is a future upgrade, breaking change, and CVE.

**When genuinely needed:** smallest API surface that solves actual problem. Boring over exciting. Many users over few. Old and maintained over new and trending. If grug can't get basic example working in 15 minutes, bad sign.

## Using

**Learn the tool deeply.** Spend real time on docs, shortcuts, config, debug modes. Two weeks learning often makes development twice faster.

**Use the tool the way the tool wants.** Fighting conventions is losing battle. Do not wrap tool in abstraction "in case we switch later." You won't. And if you do, wrapper won't help because new tool works differently.

**One tool per job.** Two ORMs is worse than one. Two state managers is demon territory.

## Wrapping Non-Grug Tools

Some tools have big brain APIs. Too many steps between grug and the thing grug wants to do.

- **Hide bad API behind good function.** One function, verb name, does the thing. Ceremony stays inside. This is a cut point.
- **Don't leak tool abstractions.** If `StreamCollectorFactoryBuilder` appears outside the wrapper, demon has escaped.
- **Log inputs and outputs at the boundary.** When tool misbehaves, grug needs to see what went in and what came out.

## Removing

- Used in one place? Inline it, delete dependency.
- Ten thousand lines for one function? Write the function.
- More debugging time than it saves? It has failed its only job.
