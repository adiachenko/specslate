---
name: spec-fortify
description: Examine a project spec after its decisions have been explored to find contradictions, gaps, accidental complexity, and other issues. Use as the final step in the spec-driven workflow to strengthen the spec before implementation begins.
---

# Spec Fortify

Use this skill for the fortification phase of the spec-driven workflow.

## Instructions

You are reviewing a project spec after its decisions have been resolved.

The user will reference a spec file. Read it in full.

This skill may also be used in two indirect ways:

- to draft a focused prompt for a model outside the current tool or session to
  review the spec through a specific lens
- to evaluate and integrate feedback produced by a model outside the current
  tool or session back into the fortification pass

When drafting such a prompt, prefer telling the user to attach or paste the
spec itself for the other model. Include any additional context explicitly, and
do not assume the other model can see the current conversation, files, or tool
state unless the user plans to pass them along.

Cross-tool review is optional. It is a probe, not a replacement for
fortification.

## Fortification Posture

Fortification is an integration pass, not an adversarial hunt for more work.
It is valid to conclude that the spec needs no changes.

Prefer the smallest change that resolves a real issue:

1. clarify a constraint, explicit assumption, or accepted-risk boundary
2. tighten verification requirements
3. add a new pending decision only when implementation would otherwise proceed
   in materially different ways
4. reopen a resolved decision only as a last resort

Reopen a decision only when at least one of these is true:

- the current text permits two materially incompatible implementations
- two resolved decisions, or a resolved decision and a stated constraint, truly
  conflict
- the spec claims a safety, trust, ownership, or delivery guarantee that the
  current text cannot actually support
- leaving the decision resolved would likely cause incorrect implementation,
  unsafe defaults, or destructive operator behavior

Do not reopen a decision merely because:

- there is residual risk the spec already accepts explicitly
- the system has a deliberate version-one limitation or non-goal
- a concern can be handled by making an implicit assumption explicit
- you can imagine a cleaner future extension

## Check For

1. **Contradictions.** Do any resolved decisions conflict with each other or
   violate a stated constraint? If a short clarification removes the
   contradiction, prefer that over reopening.

2. **Gaps.** Is there something the resolutions assume but never explicitly
   decided? If the missing piece is only an implicit assumption, accepted risk,
   or dependency, make it explicit instead of creating a new decision.

3. **Accidental complexity.** Read all resolutions as a whole. Is the total
   complexity proportional to the summary? Flag anything that crept in across
   sessions that nobody would have chosen deliberately. Do not reopen just
   because a simpler future exists; the complexity must be disproportionate to
   the stated goals as written.

4. **Security risks.** Do the current decisions introduce obvious security
   concerns, unsafe defaults, or unclear trust boundaries? Prefer adding an
   explicit constraint or assumption first. Reopen only when the current text
   would otherwise misstate the safety or trust posture.

5. **Scaling risks.** Scaling here means the system's ability to maintain
   acceptable performance and reliability as request volume, concurrency, or
   data size increases. Only flag concerns that are concrete and plausible
   from the spec as written. Explicitly accepted capacity limits are not gaps;
   only flag limits that contradict the claimed contract or are missing from
   it.

6. **Verification steps.** Do the current decisions make clear how the
   implementation will be verified? Prefer adding concrete verification
   requirements over reopening the underlying decision when that is enough.

## Update the Spec

Don't add review notes or commentary to the spec file. If the review surfaces
something actionable, apply it directly to the spec with the user's approval.
It is valid to make no changes if the spec is already coherent enough to
implement safely.

Actionable changes include:

- adding or clarifying a constraint, explicit assumption, or accepted-risk
  boundary
- tightening verification requirements
- adding a new pending decision
- reopening a resolved decision
- moving explicitly deferred nice-to-haves into `Future Ideas`
