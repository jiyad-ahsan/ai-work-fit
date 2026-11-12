# Testing

Fourteen cases live in `evals/`, one folder each, in the layout used by `claude plugin eval`. If you change the skill, re-run all of them. Twelve cover behaviors the skill must get right, and two check that it stays out of the way. The prompts deliberately use different domains from the worked examples in `SKILL.md` and `references/examples.md`, so a pass means the rule transferred and not that an example was recalled. Keep it that way when adding cases.

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
| `pricing-missing-fact` | Name the missing fact and answer both branches | Defaulting to "build the agent" because the user said "agent" |
| `tracking-number-routine` | Routine work gets a script; a model only at the parse step | Dressing a fixed mapping up as AI work |
| `weak-check-reply-rate-agent` | Test whether the check is good enough to loop against | Calling anything with a number "verifiable" and building an unattended loop |
| `prioritize-five-processes` | Rank by payoff, using volume as well as type | Ranking on type alone, or putting a twice-a-year task first |
| `refund-stakes-signoff` | Irreversible, outward-facing actions get a person's sign-off | An autonomous agent moving money |
| `rare-low-value-skip` | "Don't build it" is a legitimate answer | Inventing an architecture for a half-hour yearly task |
| `scout-undocumented-process` | An agent as a temporary scout, with a plan to replace it | Leaving the scout as the permanent system, or refusing to use one |
| `graduate-learnable-to-script` | Demote learnable work to a script once the pattern is stable | Paying for a model call forever on solved work |
| `loop-test-suite-migration` | Recommend a loop when the check is strong (a test suite), with guardrails and an owner | Talking the user out of an agent that fits |
| `loop-billing-reconciliation` | Recommend a loop when the check is strong (a reconciliation), with guardrails and an owner | Talking the user out of an agent that fits |
| `judgment-supplier-contract` | Judgment gets a single call and a named owner | Looping or drafting repeatedly with nothing to check against |
| `no-trigger-system-prompt` | Stay out of prompt-editing requests | The skill taking over a request it was not built for |
| `no-trigger-debug-agent` | Stay out of agent-debugging requests | The skill taking over a request it was not built for |

## Validation status

The suite (14 cases, 55 assertions) was run through `claude plugin eval` on 12 November 2026 with Claude Code 2.1.322: three runs per case, with and without the skill, on Sonnet 5.5 with the default Haiku judge.

| | With the skill | Without |
|---|---|---|
| Mean case score | 1.00 | 0.78 |
| Cases passing every assertion in all three runs | 14 of 14 | 7 of 14 |

These figures combine two runs. The first run failed six cases with the skill loaded. After it, `SKILL.md` gained a rule to keep answers in proportion to the request, and five graders were rewritten because they demanded particular wording rather than the behavior:

- `pricing-missing-fact/02`
- `scout-undocumented-process/02`
- both `02-recommends-loop` graders
- `tracking-number-routine/03`, now `03-model-only-for-messy-text`

Those six cases were re-run against the changed skill. The other eight were not, and their figures come from the first run against the earlier `SKILL.md`. One of the six, `compound-vendor-onboarding`, had no grader change and passed on the re-run after a judge miss in the first.

Known limits:

- Five cases score 1.00 without the skill as well, so they guard against regressions but do not show the skill adding anything.
- Most runs with the skill loaded followed its approach without calling the Skill tool. The two no-trigger cases therefore show that the skill is not misapplied, not that it does not trigger.
- The proportionality rule did not visibly shorten single-task answers in the re-run.
- The suite has not been run on Haiku or Opus.

If you run it and a case fails, please open an issue with the prompt and the response.
