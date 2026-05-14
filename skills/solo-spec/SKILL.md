---
name: solo-spec
description: Use when the user asks to add a feature, build something new, or modify behavior in a project that has `.specs/next.md`. Drives the spec loop instead of jumping to implementation. Also use when the user mentions /spec, /solo-spec-loop:spec, the spec loop, or asks to "spec out" or "slice" a piece of work.
---

# Solo Spec Loop

The **agreement** is tool-agnostic: one `.specs/next.md`, same loop, same frontmatter. This skill applies in any agent that can read the repo; hard enforcement (`solo_spec_gate.py`) only exists where the host wires PreToolUse-style hooks.

If `.specs/next.md` exists in the project root (or any parent up to the git root), the spec loop is the way to do non-trivial work in this project. Use it instead of jumping to implementation.

## Trigger

Activate this skill when:

- The user asks to add a feature, build something new, or modify behavior.
- The user mentions `/spec`, `/solo-spec-loop:spec`, "spec out", "slice", "the spec loop", or "the next slice".
- A `.specs/next.md` file exists somewhere in the cwd ancestry.

If `.specs/next.md` does not exist, the project has not opted in. Suggest the spec slash command with `init` (e.g. `/solo-spec-loop:spec init` in Claude Code) only if the user is starting non-trivial work; otherwise proceed normally.

## Loop

`explore -> slice -> approve -> build -> learn`. Optional: `init`, `status`, `ship`, `reset`.

## Hard rules

- One living file: `.specs/next.md`. No ADR forest, no per-feature folders, no research docs.
- Line budget: **target 90, soft 120, hard 140**. Past 120, shrink the slice. The hook hard-blocks source writes past 140.
- The spec is a checkpoint, not a handoff document. Capture earned understanding, not premature certainty.
- Run `explore` and `slice` in read-only / plan mode if the product has one (e.g. Cursor Plan, Claude Code plan mode). The agent should not be writing source code while the slice is still draft.
- Before implementation, the verification contract must include a runnable acceptance check (a command, not prose).
- The `solo_spec_gate.py` hook (when installed) blocks source writes until the spec body contains `status: approved`. Edits to specs, tests, docs, `.claude/`, `.cursor/`, etc. are always allowed. In hosts without that hook, enforce the same rule in behavior.
- During build, if you go past ~3 revisions on the same slice, stop. Ask the user to `/clear` and restart with only the spec and currently-edited files in context.

## Question budget

Five questions, max. Only ones that change behavior, UX, data, security, scope, or the verification contract. Everything else: propose a default in Assumptions and move on.

## What this skill does NOT do

- Generate long requirements docs.
- Create constitution / ADR / phase / task / wave directories.
- Spawn verifier subagents or run multi-agent orchestration.
- Maintain delta specs in per-change directories.

If the user wants any of those, they're picking a different framework (Spec Kit, GSD, OpenSpec). Solo Spec Loop is deliberately minimal.

## Slash command

Arguments are always: `init | explore <idea> | slice | approve | build | learn | status | ship | reset`.

**Claude Code (this plugin):** `/solo-spec-loop:spec <arguments>` — plugin id + command stem from `commands/spec.md`.

**Cursor / project-local copy:** may appear as `/spec <arguments>`. Use the name your command palette shows.

See `commands/spec.md` for full mode semantics.
