# Changelog

## 1.0.0 (August 2026)

Initial release.

- Two-stage diagnostic: classify the work (routine / learnable / verifiable / judgment), then match the architecture (script / graph / loop / single call).
- Compound workflows are split into component decisions before classification, rather than classified as one type.
- Conditional handling when the type hinges on a fact the request doesn't contain (most often whether an external check exists): the skill names the deciding fact and answers both branches instead of guessing.
- Validated against the three cases in TESTING.md.

Companion essays: [Not All AI Work Is AI Work](https://jiyad-ahsan.medium.com/not-all-ai-work-is-ai-work-23f2c9d80ce6) · [Agents, Loops, and Graphs (and When to Use Neither)](https://medium.com/@jiyad-ahsan/agents-loops-and-graphs-and-when-to-use-neither-54fdfc71aa1d)
