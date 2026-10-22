# AI Work Fit

A Claude Skill that decides who, or what, should do a piece of work. It sorts a task or workflow into one of four types, routine, learnable, verifiable, or judgment, then recommends the cheapest build that actually fits (a script, a human-drawn graph, an agent loop, or a single model call with a person staying in charge) and says where the person sits.

Most AI project disappointment comes from one mistake: reaching for the most autonomous architecture because it's the most impressive, rather than the one the work actually needs. This skill operationalizes the framework from two essays:

- [**Not All AI Work Is AI Work**](https://jiyad-ahsan.medium.com/not-all-ai-work-is-ai-work-23f2c9d80ce6), the four types, and why treating "AI work" as one thing gets both the economics and the architecture wrong.
- [**Agents, Loops, and Graphs (and When to Use Neither)**](https://medium.com/@jiyad-ahsan/agents-loops-and-graphs-and-when-to-use-neither-54fdfc71aa1d), why each type maps to a specific build, and why most things reached for as "agents" shouldn't be.

## What it does

Give it a task or workflow, in as much or as little detail as you have. It splits compound workflows into their component decisions, since most real workflows are a bundle of several types at once, then runs each through two diagnostics: what kind of work is this, and given that, what should be built and who owns what.

Version 2 adds the parts a decision-maker actually needs next:

- **A human role for every decision**, so the answer covers who stays accountable and not only what to build.
- **A test for whether a check is good enough to loop against**, so an agent isn't pointed at a noisy, slow, or gameable number.
- **Guardrails for loops**, and a rule that irreversible or outward-facing actions get a person's sign-off whatever the type.
- **A way to rank several candidate processes**, by payoff and not by type alone, including the honest answer "don't automate this".
- **Model-tier guidance**, since the four types have different economics.

## Example

**Input:** "We want AI to handle customer support end to end."

**Output:**

| Task | Type | Build | Human role |
|---|---|---|---|
| Route ticket by category | Routine | Script | Owns the exceptions queue |
| Decide whether an angry message needs escalation | Learnable | Graph | Reviews a sample of the model's calls |
| Diagnose why one customer's integration keeps failing | Verifiable | Loop (agent) | Owns the check; gets the escalation if it never resolves |
| Decide whether to comp a subscription to protect the relationship | Judgment | Single call | Decides; the model drafts |

More worked examples are in [`references/examples.md`](references/examples.md).

## Installing

Custom skills don't sync between products, so install it separately wherever you want to use it.

**Claude Code:** copy this folder into `~/.claude/skills/ai-work-fit/` (available in every project) or `.claude/skills/ai-work-fit/` inside a specific project, then restart Claude Code, or run `/skills` to confirm it loaded.

**Claude.ai:** download `ai-work-fit.zip` from the [latest release](https://github.com/jiyad-ahsan/ai-work-fit/releases/latest) and upload it under Settings > Features. Custom skills need a Pro, Max, Team, or Enterprise plan with code execution enabled, and apply to your own account only.

**Claude API:** upload the same zip through the Skills API. See the [Skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide).

The docs change often, so check the [Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) if a step doesn't match what you see.

## What's in the repo

```
SKILL.md                  the skill
references/examples.md    worked examples, loaded only when needed
evals/                    12 test cases (see TESTING.md)
TESTING.md                how to run the tests and what each one catches
CHANGELOG.md              version history
```

## Testing and versions

Version 2.0.0 ships with 12 test cases in `evals/`, including two that check the skill stays out of the way when someone just wants a prompt edited or a bug fixed. See [TESTING.md](TESTING.md) for how to run them and for the current validation status. Version history is in [CHANGELOG.md](CHANGELOG.md).

## Background

Built by [Jiyad Ahsan](https://jiyad-ahsan.medium.com), founder of Telos, a Malaysia-based AI strategy and digital transformation consultancy.

## License

MIT, see [LICENSE](LICENSE).
