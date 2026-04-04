---
name: spec-step
description: Implement the next unchecked step in a spec's implementation plan. Handles one step per invocation, verifies against the spec, and keeps the remaining plan current.
---

# Spec Step

Use this skill to implement a single step from a spec's implementation plan.

## Instructions

The user will reference a spec file. Read it in full: summary, constraints,
resolved decisions, future ideas, and the implementation plan.

Find the first unchecked step in the plan. If the user asks for a specific step
by name or number, use that one instead. If there are no unchecked steps, the
implementation is complete — report that and stop.

## Before Implementing

Review the codebase to understand the current state. Check git history from
previous steps for context on what changed and any patterns established.

Read the step's description and assumptions. Verify that the assumptions hold
against the actual codebase — if a prior step left the system in a different
state than assumed, address that before proceeding.

## Implementation

Implement the step end to end: code, wiring, tests, and documentation changes.
Work in the style and conventions established by the existing codebase and prior
steps.

Stay within the step's scope. If you notice work that belongs to a later step,
leave it for that step. If you notice work that belongs to no step, do not do
it — flag it in your report instead.

Use subagents when helpful without stopping to ask.

### Staying on Track

Do not stop at scaffolding, partial implementations, or "good progress." The
step is not done until its full description is satisfied. If tests need writing,
write them. If wiring is needed, wire it. Keep going until the step delivers
what it promises.

If you hit a blocker — a failing test you cannot resolve, a missing dependency,
an ambiguity in the spec — note it and keep implementing everything else in the
step that is not blocked by it.

## Verification

Before marking the step complete, verify:

1. **Step description.** Does the implementation deliver what the step
   describes?
2. **Constraints.** Does the implementation respect every constraint in the
   spec?
3. **Resolved decisions.** Does the implementation conform to the resolved
   decisions that are relevant to this step?
4. **Assumptions of the next step.** Will the next unchecked step's assumptions
   hold against the current state of the codebase?
5. **Tests pass.** Run the relevant test suite and confirm it passes.

If any check fails, fix the issue before proceeding. Only mark the step done
after all checks pass.

## Marking Done

Check the step's box in the implementation plan (`- [x]`).

## Updating the Remaining Plan

Implementation often reveals that remaining steps need adjustment. After
completing the current step, review the unchecked steps against the current
codebase state. If any remaining step's description or assumptions no longer
make sense, update them.

Allowed plan changes:

- rewrite the description or assumptions of an unchecked step
- merge two unchecked steps that are no longer worth separating
- split an unchecked step that has grown too large to review in one sitting
- reorder unchecked steps when dependencies have shifted
- add a new unchecked step when implementation revealed missing work
- insert a rework step ahead of the current position when a completed step
  needs revision (see **Revisiting Earlier Steps** below)

Never modify a checked step. The checked steps are history — their record stays
as-is and any rework gets its own new step.

## Revisiting Earlier Steps

If implementation reveals that a completed step needs rework — a wrong
assumption baked in, a structural choice that doesn't hold — do not go back and
redo it within this invocation. Instead, insert a new unchecked step into the
plan that describes the specific rework needed, placed before the next step that
depends on it. Report this to the user. The next `spec-step` invocation picks
up the rework, keeping each change reviewable on its own.

## Implementation Contradictions

If implementation reveals that a resolved decision in the spec is wrong,
incomplete, or impractical, do not silently deviate and do not block on it.
Proceed with the pragmatic choice that keeps the current step deliverable.

Record the contradiction in the spec so it is not lost across sessions, but do
not automatically reopen the decision or convert it back into active
exploration.

If the contradiction does not materially change the spec's user-visible
contract, trust boundary, ownership boundary, or accepted-risk posture, add a
brief note under a dedicated `## Implementation Contradictions` section and
continue. Use this format:

```md
## Implementation Contradictions

- **D04 / Step N: <Short title>**
  <Brief description of what the spec said, what was implemented instead, and
  why the divergence happened.>

- **D07 / Step N: <Short title>**
  <Brief description of what the spec said, what was implemented instead, and
  why the divergence happened.>
  Follow-up: <reconciliation step added to the implementation plan, if needed>
```

If the contradiction does materially affect the spec's meaning or would make
future steps plan against something false, insert a new unchecked
reconciliation/rework step into the implementation plan before any later step
that depends on the outdated assumption. Keep the original resolved decision
intact until that reconciliation step is handled explicitly.

Always report the contradiction clearly: what the spec said, what was done
instead, why, what was recorded in the spec, and whether a follow-up plan step
was added.

## Report

After completing the step (or after exhausting implementable work within it),
report:

- what was implemented
- verification results
- any plan changes made to remaining steps
- any rework steps inserted, with reasons
- any decision contradictions, with what was done instead and why
- any blockers, with exact reasons

Keep it concise. If nothing was blocked, contradicted, or changed in the plan,
say so briefly with verification results only.
