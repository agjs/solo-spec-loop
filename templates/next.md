---
status: draft # draft | approved
slice: ""
approved_at: ""
---

# Next slice

## Purpose
One or two sentences: who is served, and what should become true?

## One-session exit condition
The smallest observable outcome that makes this session worth it.

- [ ]

## Event sketch
Keep it tiny. Use this to avoid vague specs.

- Trigger / command:
- State or rule that changes:
- Observable result / event:
- Who sees it:

## Slice
What we will build now, in the smallest vertical cut.

-

## Questions that matter
Only questions that change behavior, UX, data, security, scope, or verification.

- [ ]

## Assumptions
Defaults we proceed with unless corrected.

-

## Verification contract
The guardrails for iteration.

- Must remain true:
- Must not happen:
- Acceptance check (a runnable command, not prose):
  - e.g. `bun test path/to.test.ts -t "case name"` or `pnpm e2e e2e/widget.spec.ts`

## Not doing
Protect the slice from becoming a project.

-

## Files likely touched
If this list explodes, the slice is too big.

-

## Build notes
3-5 bullets after building: changed, checked, learned, follow-up.
