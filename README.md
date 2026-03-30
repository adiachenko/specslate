# Specslate

Skills for a lightweight spec-driven workflow to draft, refine, and strengthen the spec for a project or feature.

Designed for decision-heavy work where early choices shape the outcome, such as greenfield projects, major features, or architectural changes with real tradeoffs. Not intended for straightforward, execution-ready tasks where you mostly just need a fast plan and momentum.

Read the skill descriptions before using this workflow. The sections below shed light on why that matters.

## 🤔 Why Use It

### Compared to Planning Mode

Built-in planning modes in Codex and Claude Code follow a single-shot pattern: describe what you want, answer a couple of questions, get a plan, approve or restart. The AI reasons about everything at once, which produces shallow coverage of individual choices and leaves little room for real exploration.

Instead, this workflow produces a document that first only maps out open questions and constraints. You then resolve each question **one at a time across separate sessions**, using the file as a persistent state machine for the process. Each decision can be explored as deeply as it deserves before it all comes together in the final comprehensive plan.

### Compared to Other Spec-Driven Workflows

Dedicated spec workflows typically add structure through templates, required fields, and multi-stage review gates. In practice, that rigidity works against you: it constrains the exploration to what the workflow anticipated, produces boilerplate where real thinking should be, and makes the process feel like filling out forms. This workflow strips it down to a single Markdown file and 3 helper prompts — just enough structure to serve the thinking, not replace it.

It aims to stay as thin as possible. Expect it to get simpler over time, not more complex.

## 🚫 What This Is Not

This is not a workflow where you hand the model a spec and then passively answer whatever questions it asks. The point is to use the model as a thinking partner, not as a form wizard.

You should proactively steer the exploration:

- push back when a question feels premature, irrelevant, or over-engineered
- redirect the discussion toward the tradeoff you actually care about
- state preferences, constraints, and instincts even when the model did not ask
- suggest additional decisions that deserve explicit exploration
- explicitly defer nice-to-have ideas instead of leaving them half-decided

One common failure mode is that the agent may not proactively move the workflow forward on its own.

## 🛠️ Installation

```bash
npx skills add adiachenko/specslate
```

## 🔄 Updating

To update the installed skill package:

```bash
npx skills update -g
```

## 🚀 Usage

Use these skills in order:

> Examples use Codex syntax with `$skill`. In Claude Code, replace `$` with `/`.

1. Use `spec-discover` to create the draft of a spec from a project idea under `.specslate/`

   Example:

   ```text
   $spec-discover Build an MCP server for calendar management
   ```

2. Use `spec-explore` to work through a single open decision in a drafted spec.

   As a decision settles, record not just the chosen direction but also the
   assumptions, accepted risks, dependencies, and trust / ownership /
   operational boundaries that make it safe.

   Example:

   ```text
   $spec-explore D04 in @.specslate/260323_calendar_mcp.md
   ```

3. Use `spec-fortify` to review the spec after the main decisions have been explored.

   `spec-fortify` is an integration pass, not an adversarial search for more
   work. It should prefer clarifying constraints or assumptions over reopening
   decisions, and it may validly conclude that no changes are needed.

   Example:

   ```text
   $spec-fortify @.specslate/260323_calendar_mcp.md
   ```

4. You'll often need to use `spec-explore` again to work through decisions added or re-opened by `spec-fortify`, then repeat that loop until you need no more fortification passes.

## Bonus Tip

`spec-fortify` can also help draft a focused prompt for another model to review
a spec, or fold that model's feedback back into the fortification pass.

Keep the review narrow and lens-specific for best results. Focus on no more than
one concern at a time, such as accidental complexity, throughput, testing, etc.

When using this path, attach or paste the spec for the external model rather
than trying to restate it, and include any extra context explicitly.

Examples:

```text
$spec-fortify Draft a GPT-5.4 Pro review prompt focused on accidental complexity for @.specslate/260323_calendar_mcp.md
```

```text
$spec-fortify Integrate this GPT-5.4 Pro feedback into @.specslate/260323_calendar_mcp.md
```
