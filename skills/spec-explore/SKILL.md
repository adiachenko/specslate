---
name: spec-explore
description: Explore a single unresolved decision in an existing project spec by surfacing options, tradeoffs, assumptions, and unknowns without forcing a premature choice. Use as the second step in the spec-driven workflow to deepen and refine the spec one decision at a time.
---

# Spec Explore

Use this skill for the exploration phase of the spec-driven workflow.

## Instructions

You are exploring a single decision in an existing project spec.

The user will reference a decision, by ID or topic, and a spec file. Read the
full spec file first. Treat resolved decisions as additional constraints.

## Exploration

This is a deep exploration, not a forced choice. Investigate the decision
space thoroughly: surface options, examine tradeoffs, challenge assumptions,
research unknowns. Follow wherever the conversation leads.

A decision may take multiple sessions to reach its final form. At the end of
each session, write your findings into the decision section of the spec file so
the next session can pick up where this one left off.

## Decision Scrutiny Protocol

Use this as a baseline for evaluating any option that surfaces during
exploration:

### 1. Justify the Decision

Start boring and treat every additional layer, service, or abstraction as a
cost that needs a hard justification, not just a potential benefit.

The goal is the thinnest possible version that works end to end. "Industry
standard" and "typical for this kind of project" are not justifications, only
stated goals are.

Ask: "What is the cost of adding this later if it turns out we need it?" Then
stress-test from both directions:

- "If I include this and it turns out unnecessary, what did it cost me?"
  (e.g. complexity, time, coupling, maintenance burden)
- "If I skip this and it turns out I needed it, what does recovery look like?"
  (e.g. hotfix at 2 AM? trivial follow-up PR? painful data migration?)

Include now if skipping is disproportionately painful to recover from.
Otherwise, defer. When in doubt, do less.

### 2. Simplicity Challenge

Ask: "Is there a way to achieve the same outcome with fewer moving parts, fewer
abstractions, or by leveraging what already exists?"

- Prefer convention over configuration.
- Prefer stdlib/framework builtins over custom implementations.
- Prefer a single well-known dependency over a clever composition of three.
- Prefer one file that does the thing over five files that organize the doing
  of it.

If you find yourself reaching for a pattern, ask: "Am I solving a problem I'm
looking at, or a problem I'm imagining?"

### 3. Roads Not Taken

For each rejected option that had a simpler alternative, write one sentence
explaining why the simpler option didn't cut it. If you can't write that
sentence, take the simpler option.

### Ground Rules

- "We might need this later" is not a justification. Solve what you've been
  asked to build.
- If two options are roughly equivalent, pick the one with fewer concepts to
  explain to a new reader.
- If you catch yourself writing "this adds flexibility" without naming a
  concrete scenario, that's a smell. Cut or defer.
- Reversibility matters: prefer decisions that are cheap to change over ones
  that are "optimal" but lock you in.

## Updating the Spec

As the exploration progresses, update the decision section in the spec file
whenever the user makes a call on something: a direction chosen, an option
ruled out, a constraint discovered. Keep it current so the next session can
pick up where this one left off.

When the user considers the decision settled, mark it ✅ in the table and make
sure the decision section reflects the final resolution clearly enough for the
next session to treat it as a constraint.
