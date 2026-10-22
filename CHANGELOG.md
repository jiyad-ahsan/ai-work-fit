# Changelog

## 2.0.0 (October 2026)

The skill now answers who stays accountable for each piece of work, not only what to build, and it covers the situations the first version only gestured at.

### Added
- **Human role** for every decision: in the build table, in the output format, and in the worked examples. Machine end to end for routine work, sample review for learnable, a person who owns the check for verifiable, a person who decides for judgment.
- **Check quality.** Before recommending a loop, test the check on four counts: independent of the agent, fast and cheap enough for every iteration, faithful to the real goal and not a proxy, and conclusive within the time the loop runs. A weak check means no loop.
- **Loop guardrails.** Iteration and spend caps, least access, a stop-and-escalate rule, and logging.
- **Stakes override.** Irreversible, regulated, or outward-facing actions get a person's sign-off whatever the type. This changes who, not what type.
- **Ranking.** A method for prioritizing several candidates by payoff against build effort, using how often a task runs and not type alone. The skill's description always promised this and the first version did not deliver it.
- **"Don't build it."** An explicit gate for rare or cheap tasks, and `None, leave it manual` as a valid build.
- **Graduation.** A concrete test for when learnable work should be demoted to a script, and the reverse case where routine work has started to drift.
- **Model tier** per type: none or smallest for routine and learnable work, the strongest affordable for verifiable work, one strong call for judgment.
- **Definitions** of script, graph, loop, and single call, including that "graph" means a human-drawn workflow and not a graph framework or a knowledge graph.
- `references/examples.md` with seven worked examples: month-end close, restaurant operations with a ranking, hiring, a loop with a weak check, a task not worth automating, a scout agent, and a stakes override.
- Twelve test cases in `evals/`, including two that check the skill does not trigger on prompt editing or agent debugging.
- A line in the description saying what the skill is not for.

### Changed
- Routine work with messy input now says in `SKILL.md` that one model call at the parse step is fine (this was only in the tests before).
- The missing-fact rule now covers any deciding fact, not only whether a check exists, and ends by asking for that one thing when it is quick to supply.
- The output format is now identical in `SKILL.md`, the README, and the tests (one table format with a Human role column; previously the tests used a three-column table the skill did not describe).
- Install instructions in the README now match the current docs: Claude Code paths, the Claude.ai zip upload and plan requirements, the API route, and the fact that custom skills do not sync across products.
- `TESTING.md` rewritten around the case folders, with a plain statement of the current validation status.

### Compatibility
No breaking changes. The four types, the two diagnostics, the conditional answer for a missing fact, and the compound-workflow split work as before.

## 1.0.0 (August 2026)

Initial release.

- Two-stage diagnostic: classify the work (routine / learnable / verifiable / judgment), then match the architecture (script / graph / loop / single call).
- Compound workflows are split into component decisions before classification, rather than classified as one type.
- Conditional handling when the type hinges on a fact the request doesn't contain (most often whether an external check exists): the skill names the deciding fact and answers both branches instead of guessing.
- Validated against the three cases in TESTING.md.

Companion essays: [Not All AI Work Is AI Work](https://jiyad-ahsan.medium.com/not-all-ai-work-is-ai-work-23f2c9d80ce6) · [Agents, Loops, and Graphs (and When to Use Neither)](https://medium.com/@jiyad-ahsan/agents-loops-and-graphs-and-when-to-use-neither-54fdfc71aa1d)
