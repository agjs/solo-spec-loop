---
description: Solo founder rolling-wave spec loop: init, explore, slice, approve, build, learn, status, ship, reset.
allowed-tools: Read, Write, Edit, MultiEdit, Bash, Glob, Grep, LS
argument-hint: init | explore <idea> | slice | approve | build | learn | status | ship | reset
---

You are running Solo Spec Loop: a lightweight workflow for one founder using AI as a collaborator, not an enterprise spec factory.

Command: $ARGUMENTS

Principles:
- The spec is a conversation checkpoint, not a handoff document.
- Work in vertical slices. Small batch size is non-negotiable.
- Capture only earned understanding. Mark uncertain things as assumptions or questions.
- Prefer a tiny event sketch over long prose: Trigger -> change -> observable result.
- Define verification before implementation: what must be true, what must never happen, and the exact runnable acceptance check.
- Do not generate feature folders, ADR files, research docs, diagrams, or extra Markdown unless the human explicitly asks.
- Use exactly one living file: `.specs/next.md`.
- Line budget: **target 90, soft warning 120, hard stop 140**. Past 120, shrink the slice instead of adding detail. The gate hard-blocks source writes past 140.

Question budget:
- Ask at most 5 questions.
- Ask only questions where the answer changes behavior, UX, data, security, scope, or the verification contract.
- For everything else, propose a default in Assumptions and move on.

Plan-mode pairing:
- Run `explore` and `slice` in Cursor Plan mode or Claude Code plan mode. The agent should not be writing source while the slice is still draft. The PreToolUse gate enforces this for source files; plan mode keeps you honest about everything else.

Modes:

`init`
1. If `.specs/next.md` already exists, do nothing and print its current `status:` and `slice:`.
2. Otherwise, create `.specs/` and copy the bundled template into `.specs/next.md`.
   - Bundled template: `${CLAUDE_PLUGIN_ROOT}/templates/next.md` if available, else inline the canonical structure (status frontmatter, Purpose, One-session exit condition, Event sketch, Slice, Questions, Assumptions, Verification contract, Not doing, Files likely touched, Build notes).
3. Print: "Spec loop active. Next: `/spec explore <idea>`."

`explore <idea>`
1. Inspect only enough repo context to avoid obvious nonsense.
2. Update `.specs/next.md` as a lightweight discovery artifact.
3. Fill: Purpose, One-session exit condition, Event sketch, Slice, Questions, Assumptions, Verification contract, Not doing, Files likely touched.
4. End with either up to 5 important questions or: "No blocking questions. Proposed defaults are in Assumptions."
5. Do not write implementation code.

`slice`
1. Read `.specs/next.md` and relevant code.
2. Reduce scope until the slice can be built, reviewed, and reverted in one focused session.
3. Ensure the Event sketch describes one user/system trigger and one observable result.
4. Ensure the Verification contract includes:
   - must remain true
   - must not happen
   - acceptance check (a runnable command, not prose)
5. Remove stale detail instead of adding more sections.
6. Do not write implementation code.

`approve`
Only if the human has explicitly approved this exact slice and verification contract.
Set frontmatter `status: approved` and set `approved_at` if date/time is available.
Do not implement.

`build`
1. Read `.specs/next.md` first.
2. Proceed only if `status: approved`.
3. Implement only the approved slice.
4. Prefer tests/checks that prove the Verification contract. If test skeletons are missing and the change is non-trivial, create them first.
5. Run the most relevant checks.
6. Update Build notes in 3-5 bullets: changed, checked, learned, follow-up.
7. If a material ambiguity appears, stop and ask instead of expanding scope.
8. Context-refresh hint: if you've gone past about 3 revisions on the same slice, stop and ask the user to `/clear`. Restart the session with only `.specs/next.md` and the files you're currently editing. Long sessions degrade attention; manual context isolation beats spinning subagents.

`learn`
Update only Build notes, Assumptions, Verification contract, or Not doing with what reality taught us.
If a reusable convention is worth saving, propose one line and ask before editing project docs.
Do not create ADRs unless explicitly asked.

`status`
Read `.specs/next.md` frontmatter and print:
- `status:` (draft | approved)
- `slice:` (one-line summary)
- `approved_at:` and the rough number of days since approval (if set)
- Current line count vs. the 90 / 120 / 140 budget
- The verification contract's acceptance check, verbatim

Do not modify the spec.

`ship`
1. Read `.specs/next.md`.
2. Proceed only if `status: approved`.
3. Run the acceptance check from the Verification contract verbatim. If it is not a runnable command, stop and ask the user to tighten the contract first.
4. Append a single Build notes line: `<YYYY-MM-DD> ship: <pass|fail> -- <command>`.
5. On pass, prompt: "Ship verified. Run `/spec learn` to capture earned understanding, or `/spec reset` to start the next slice."
6. On fail, do not modify anything else; print the failing command's output and stop.

`reset`
Archive nothing by default. Clear `.specs/next.md` back to the template for the next slice. Before resetting, summarize any unresolved follow-ups in 3 bullets max.
