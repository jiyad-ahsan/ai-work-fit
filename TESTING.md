# Testing

Twelve cases live in `evals/`, one folder each, in the layout used by `claude plugin eval`. If you change the skill, re-run all of them. Ten cover behaviors the skill must get right, and two check that it stays out of the way.

## Running them

**By hand (works anywhere).** For each case, open `evals/<case>/prompt.md`, give Claude the prompt with the skill installed, and check the response against the statements in `evals/<case>/graders/`. Each statement is a single pass/fail check.

**With Claude Code** (v2.1.269 or later), from the repo root:

```
claude plugin eval . --allow-tools Read,Glob,Grep,Skill
```

It runs every case with and without the skill and writes a report under `evals/results/`. These are real model runs and are billed.

## What each case catches

| Case | Behavior it protects | Failure it catches |
|---|---|---|
| `compound-vendor-onboarding` | Split a bundle into decisions, classify each, name a human role for each | Classifying the whole bundle as one type |
| `budget-split-missing-fact` | Name the missing fact and answer both branches | Defaulting to "build the agent" because the user said "agent" |
| `tracking-number-routine` | Routine work gets a script; a model only at the parse step | Dressing a fixed mapping up as AI work |
| `weak-check-conversion-agent` | Test whether the check is good enough to loop against | Calling anything with a number "verifiable" and building an unattended loop |
| `prioritize-five-processes` | Rank by payoff, using volume as well as type | Ranking on type alone, or putting a twice-a-year task first |
| `refund-stakes-signoff` | Irreversible, outward-facing actions get a person's sign-off | An autonomous agent moving money |
| `rare-low-value-skip` | "Don't build it" is a legitimate answer | Inventing an architecture for a twenty-minute quarterly task |
| `scout-undocumented-process` | An agent as a temporary scout, with a plan to replace it | Leaving the scout as the permanent system, or refusing to use one |
| `graduate-learnable-to-script` | Demote learnable work to a script once the pattern is stable | Paying for a model call forever on solved work |
| `judgment-supplier-contract` | Judgment gets a single call and a named owner | Looping or drafting repeatedly with nothing to check against |
| `no-trigger-system-prompt` | Stay out of prompt-editing requests | The skill taking over a request it was not built for |
| `no-trigger-debug-agent` | Stay out of agent-debugging requests | The skill taking over a request it was not built for |

## Validation status

Version 2.0.0 was checked against all twelve cases (45 assertions, all passing) by the same assistant that wrote the changes. That is a sanity check, not an independent validation. It has not yet been run through `claude plugin eval`, and it has not been tested across Haiku, Sonnet, and Opus, which behave differently with the same instructions. If you run the suite and a case fails, please open an issue with the prompt and the response.
