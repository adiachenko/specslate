# Specslate

> This workflow is **work in progress**.

Skills for a lightweight spec-driven workflow to draft, refine, and strengthen the spec for a project or feature.

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

## 🚀 Usage

Use these skills in order:

> Examples use Codex syntax with `$skill`. In Claude Code, replace `$` with `/`.

1. `spec-discover`
   Use to create the first version of a spec from a project idea under `.specslate/` using the `YYMMDD_<topic_slug>.md` naming pattern.

   Example:

   ```text
   $spec-discover Build an MCP server for calendar management.
   ```

   After saving the spec, it will also automatically configure local ignore rules for `.specslate/` so specs stay out of git by default while remaining available to Codex `@` file references.

2. `spec-explore`
   Use to work through a single open decision in an existing spec.

   Example:

   ```text
   $spec-explore D04 in @.specslate/260323_calendar_mcp.md
   ```

3. `spec-fortify`
   Use to review the spec after the main decisions have been explored.

   Example:

   ```text
   $spec-fortify @.specslate/260323_calendar_mcp.md
   ```
