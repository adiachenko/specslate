# 📘 Specslate

> This workflow is **work in progress**.

Skills for a lightweight spec-driven workflow to draft, refine, and strengthen the spec for a project or feature.

## 🤔 Why This Workflow

### Compared to Planning Mode

Built-in planning modes in Codex and Claude Code follow a single-shot pattern: describe what you want, get a plan, approve or restart. The AI reasons about everything at once, which produces shallow coverage of individual choices and leaves little room for real exploration.

Instead, this workflow produces a document that first only maps out open questions and constraints. You then resolve each question **one at a time across separate sessions**, using the file as a persistent state machine for the process. Each decision can be explored as deeply as it deserves before it all comes together in the final comprehensive plan.

### Compared to Other Spec-Driven Workflows

Dedicated spec workflows typically add structure through templates, required fields, and multi-stage review gates. In practice, that rigidity works against you: it constrains the exploration to what the workflow anticipated, produces boilerplate where real thinking should be, and makes the process feel like filling out forms. This workflow uses plain Markdown and a small set of focused skills, so the structure serves the thinking rather than replacing it.

It aims to stay as thin as possible. Expect it to get simpler over time, not more complex.

## 🛠️ Installation

```bash
npx skills add adiachenko/spec-driven-workflow
```

## 🚀 Usage

Use these skills in order:

> Examples use Codex syntax with `$skill`. In Claude Code, use the same input but replace `$spec-discover`, `$spec-explore`, and `$spec-fortify` with `/spec-discover`, `/spec-explore`, and `/spec-fortify` respectively.

1. `spec-discover`
   Use to create the first version of a spec from a project idea.

   After saving the spec, it should offer to configure local ignore rules for
   `specs.local` so specs stay out of git while remaining available to Codex
   `@` file references.

   Example:

   ```text
   $spec-discover Build an MCP server for calendar management.
   ```

2. `spec-explore`
   Use to work through one open decision in an existing spec.

   Example:

   ```text
   $spec-explore D04 in @specs.local/001_calendar-mcp.md
   ```

3. `spec-fortify`
   Use to check the spec after the main decisions have been explored.

   Example:

   ```text
   $spec-fortify @specs.local/001_calendar-mcp.md
   ```
