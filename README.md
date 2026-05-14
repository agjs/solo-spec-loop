# Solo Spec Loop

One file, one loop, one gate. Stops AI agents from sprinting ahead of you.

## What it is

A workflow for solo founders using AI to build:

```text
explore → slice → approve → build → learn
```

Lives in one file per project: `.specs/next.md`.

## The problem

You ask an AI for a small feature. You get a folder tree, an ADR forest, and 800 lines of code for something nobody designed. Reviewing it is slower than writing it yourself.

Solo Spec Loop forces a checkpoint: the agent can't touch source until you've explicitly approved a small, vertical slice with a runnable acceptance check.

## 30-second start (Claude Code)

In Claude Code, run:

```text
/plugin marketplace add agjs/solo-spec-loop
/plugin install solo-spec-loop@agjs-plugins
```

Then in any project:

```text
/spec init             # creates .specs/next.md
/spec explore <idea>   # discovery, no code
/spec slice            # shrink to one vertical cut
/spec approve          # only when you mean it
/spec build            # now the agent can write code
```

The hook blocks source writes while the spec is `draft`. Edits to specs, tests, docs, and config stay allowed. Done.

## Other tools

Cursor: install from this repo (`.cursor-plugin/`).

Anything else (Codex, Gemini, Aider, Copilot, plain ChatGPT): copy `templates/next.md` to `.specs/next.md`, paste `rules/spec-loop.mdc` into your `AGENTS.md`. Optionally wire `hooks/solo_spec_gate.py` into a pre-write hook if your tool has one.

## What's in the repo

```text
hooks/solo_spec_gate.py    the portable gate (~160 lines stdlib Python)
templates/next.md          the spec template
commands/spec.md           /spec slash command
rules/spec-loop.mdc        Cursor always-apply rule
skills/solo-spec/          Claude Code skill
tests/                     stdlib-only tests for the gate
```

## Tests

```bash
python3 tests/test_solo_spec_gate.py
```

## What this is not

Not [Spec Kit](https://github.com/github/spec-kit). Not [GSD](https://github.com/gsd-build/get-shit-done). Not [OpenSpec](https://github.com/Fission-AI/OpenSpec). No constitution, no per-feature folders, no multi-agent orchestration. If you want any of that, pick one of those.

## License

MIT.
