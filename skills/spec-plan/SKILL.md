---
name: spec-plan
description: Decompose a fortified spec into an ordered sequence of implementation steps, each scoped as a single reviewable unit of work. Use after fortification to bridge the gap between spec and code.
---

# Spec Plan

Use this skill to produce an implementation plan from a fortified spec.

## Instructions

The user will reference a spec file. Read it in full, including resolved
decisions, constraints, and future ideas.

Before planning, check the decisions table. If any decision is not marked ✅,
stop and tell the user which decisions are still unresolved. Do not produce a
plan against an incomplete spec — the plan would have to guess at unresolved
decisions, and guessing is not the planner's job.

Review the codebase to understand the current state: existing code, tests,
configuration, and any scaffolding that may already be in place. The plan must
account for what exists, not assume a blank slate.

## Planning

Decompose the spec into an ordered sequence of implementation steps. Each step
is a vertical slice — a coherent unit of functionality implemented end to end
(model, wiring, tests, docs) that can be reviewed in isolation.

Vertical means: one step should not be "add all models" followed by "add all
routes." Each step delivers a working, testable increment of the system. A
reviewer reading only that step's diff and the spec should be able to judge
whether it was done correctly.

Order steps so that each one builds on a working state left by the previous
step. Earlier steps should establish foundations that later steps extend, not
the reverse.

### Step Scope

A well-scoped step:

- delivers one coherent piece of functionality, not a grab bag
- is small enough to review in a single sitting
- leaves the system in a working state (tests pass, no broken imports)
- has a description clear enough that someone familiar with the spec knows
  exactly what it covers without reading the code

A step that touches more than two or three major areas of the system is
probably too large. A step that only adds a type definition or an empty
migration with no behavior is probably too small.

### Step Format

Each step has:

- a short title
- a description of what it delivers (one to three sentences)
- assumptions about what is already in place when this step starts

Assumptions are informal. They describe the expected state of the system, not
formal references to other steps or decision IDs. "Message model and migration
exist" is enough — not "depends on step 2 which implements D03."

### What Does Not Belong in a Step

- Spec decisions or constraint changes. The plan is a read-only consumer of the
  spec. If the spec needs updating, that happens outside this skill.
- Implementation details like specific class names, method signatures, or file
  paths. Those are decisions for the implementer, not the planner.
- Steps for work that is in `Future Ideas`. The plan covers the current spec
  scope only.

## Write the Plan

Append an `## Implementation Plan` section to the spec file, after `Future
Ideas`. Use this format:

```md
## Implementation Plan

- [ ] **Step 1: <Title>**
  <Description of what this step delivers.>
  Assumes: <what must be in place before this step starts>

- [ ] **Step 2: <Title>**
  <Description of what this step delivers.>
  Assumes: <what must be in place before this step starts>

...
```

If the spec already has an `## Implementation Plan` section with no checked
steps, replace it. If any steps are already checked, stop and tell the user —
re-planning would discard completed work. The user can either manually adjust
the remaining steps or clear the checkboxes to start fresh.

## Rules

- Every constraint and resolved decision in the spec must be covered by at
  least one step. If something in the spec has no home in the plan, that is a
  gap — add a step or flag it to the user.
- Do not plan work outside the spec scope. If you notice the spec is missing
  something, tell the user rather than silently adding a step for it.
- Prefer fewer, well-scoped steps over many granular ones. The goal is
  reviewable increments, not a task tracker.
- The plan is a starting point, not a contract. Steps will be revised as
  implementation reveals new information. Optimize for clarity now, not
  durability.
