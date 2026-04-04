---
name: spec-plan
description: Decompose a fortified spec into an ordered sequence of implementation steps, each scoped as a single reviewable unit of work. Use after fortification to bridge the gap between spec and code.
---

# Spec Plan

Use this skill to produce an implementation plan from a fortified spec.

## Instructions

The user will reference a spec file. Read it in full, including resolved
decisions, constraints, and future ideas.

If an unresolved item appears non-blocking because different outcomes would not
materially change the implementation contract, tell the user the spec likely
needs cleanup before planning: convert that item into an explicit assumption,
accepted risk, limitation, or `Future Ideas` entry instead of planning around it.

Before planning, check the decisions table. If any decision is not marked ✅,
stop and tell the user which decisions are still unresolved. Do not produce a
plan against an incomplete spec — the plan would have to guess at unresolved
decisions, and guessing is not the planner's job.

Review the codebase to understand the current state: existing code, tests,
configuration, and any scaffolding that may already be in place. The plan must
account for what exists, not assume a blank slate.

## Planning

Decompose the spec into an ordered sequence of implementation steps.

Start by identifying the thinnest end-to-end behaviors implied by the spec.
Plan around those behaviors, not around subsystems, layers, or broad
infrastructure buckets.

A true vertical slice reaches one observable outcome: something a user,
developer, operator, or test can now do, run, or verify that was not possible
before. A step may include small amounts of shared foundation, but only the
minimum required for that outcome. If a step mostly leaves behind prerequisites
and no meaningful new behavior, it is not a good slice yet.

Order steps so that executable or otherwise observable value appears early.
Prefer the smallest working path through a core behavior before infrastructure
expansion.

Extract shared foundation ahead of a behavior slice only when at least two
immediately upcoming slices need it and the extracted foundation is itself
small enough to review comfortably.

### Step Scope

A well-scoped step:

- delivers one observable capability, or one clean extension of an already
  working capability
- is small enough to review in a single sitting
- leaves the system in a working state (tests pass, no broken imports)
- has a description clear enough that someone familiar with the spec knows
  exactly what it covers without reading the code

Split a step when any of the following is true:

- it introduces more than one top-level command, runtime path, or operator
  workflow
- it bundles a happy path with retries, recovery, coordination, or failure
  handling that could be reviewed separately
- it spans multiple adapters, backends, or integrations when one step could
  establish the contract and later steps could extend it
- it mixes core behavior with optional optimization, batching, migration, or
  broad cleanup
- its description is mostly a list of concerns joined by "and"

A step that only adds a type definition, empty migration, or other prerequisite
with no meaningful new behavior is probably too small.

### Step Format

Each step has:

- a short title naming the capability delivered, not just the subsystem touched
- a description of what it delivers (one to three sentences), focused on the
  observable behavior or extension unlocked by the step
- assumptions about what is already in place when this step starts

Assumptions are informal. They describe the expected state of the system, not
formal references to other steps or decision IDs. "Message model and migration
exist" is enough — not "depends on step 2 which implements D03." Assumptions
should name only immediate prerequisites. If a step assumes most of the
architecture is already in place, the step is probably too large or ordered
too late.

### What Does Not Belong in a Step

- Spec decisions or constraint changes. The plan is a read-only consumer of the
  spec. If the spec needs updating, that happens outside this skill.
- Implementation details like specific class names, method signatures, or file
  paths. Those are decisions for the implementer, not the planner.
- Steps for work that is in `Future Ideas`. The plan covers the current spec
  scope only.

## Write the Plan

Append an `## Implementation Plan` section at the very end of the spec file,
after all other sections including individual decision details. Use this
format:

```md
---

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
- Every step should carry its own proof and support work. Put the tests, docs,
  observability, and operator guidance for a behavior in the same step that
  introduces that behavior whenever they naturally belong there.
- Do not plan work outside the spec scope. If you notice the spec is missing
  something, tell the user rather than silently adding a step for it.
- Prefer the smallest number of genuinely reviewable slices over the fewest
  possible steps. When deciding between one broad step and two clear slices,
  choose two.
- Adjacent steps may collectively cover one broad area. One step does not need
  to absorb every related concern at once as long as each step is reviewable on
  its own.
- Do not create a final catch-all step for testing, docs, observability, or
  operator guidance. A separate final step is only appropriate when the work is
  itself a standalone harness or release concern with no natural earlier home.
- The plan is a starting point, not a contract. Steps will be revised as
  implementation reveals new information. Optimize for clarity now, not
  durability.

## Final Check

Before writing the plan, sanity-check it:

- Does each step unlock one clearly observable capability, or one crisp
  extension of a capability?
- Would a reviewer be comfortable reviewing that step's diff in one sitting?
- Do the first one or two steps produce real observable value instead of only
  prerequisites?
- Are tests, docs, and operational proof attached to the step that introduces
  the behavior?
- Does any step bundle multiple commands, modes, adapters, or core behavior
  plus optimization? If yes, split it.
- If someone read only that step's diff and the spec, would the acceptance
  question be obvious?
