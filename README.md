# AI Work Fit

A Claude Skill that classifies a task or workflow into one of four types, routine, learnable, verifiable, or judgment, and recommends the cheapest architecture that actually fits: a script, a human-drawn graph, an agent loop, or a single model call with a human staying in charge.

Most AI project disappointment comes from one mistake: reaching for the most autonomous architecture because it's the most impressive, rather than the one the work actually needs. This skill operationalizes the framework from two essays:

- [**Not All AI Work Is AI Work**](https://jiyad-ahsan.medium.com/not-all-ai-work-is-ai-work-23f2c9d80ce6), the four types, and why treating "AI work" as one thing gets both the economics and the architecture wrong.
- [**Agents, Loops, and Graphs (and When to Use Neither)**](https://medium.com/@jiyad-ahsan/agents-loops-and-graphs-and-when-to-use-neither-54fdfc71aa1d), why each type maps to a specific build, and why most things reached for as "agents" shouldn't be.

## What it does

Give it a task or workflow, in as much or as little detail as you have, and it walks through two short diagnostics: what kind of work is this, and given that, what should actually get built. It splits compound workflows into their component decisions rather than classifying a whole process as one thing, since most real workflows are a bundle of several types at once.

## Example

**Input:** "We want AI to handle customer support end to end."

**Output:**

| Task | Type | Build |
|---|---|---|
| Route ticket by category | Routine | Script |
| Decide whether an angry message needs escalation | Learnable | Graph |
| Diagnose why one customer's integration keeps failing | Verifiable | Loop (agent) |
| Decide whether to comp a subscription to protect the relationship | Judgment | Single call |

## Installing

**Claude Code:** copy the `ai-work-fit/` folder into `~/.claude/skills/` (available in every project) or `.claude/skills/` inside a specific project, then restart Claude Code, or run `/skills` to confirm it loaded.

**Claude.ai / Claude apps:** upload `SKILL.md` (or a zip of this folder) wherever your plan supports custom skill uploads. See [docs.claude.com](https://docs.claude.com) for the current steps, this has changed recently and it's worth checking you have the latest flow.

## Testing and versions

The skill was validated against three cases covering its core behaviors, splitting compound workflows, refusing to guess when the classification hinges on a missing fact, and not inflating routine work into AI work. The cases and expected outputs are documented in [TESTING.md](TESTING.md); if you modify the skill, re-run them. Version history is in [CHANGELOG.md](CHANGELOG.md).

## Background

Built by [Jiyad Ahsan](https://jiyad-ahsan.medium.com), founder of Telos, a Malaysia-based AI strategy and digital transformation consultancy.

## License

MIT, see [LICENSE](LICENSE).
