---
name: ai-work-fit
description: Decides who or what should do a task or workflow, and how to build it. Sorts each decision into routine, learnable, verifiable, or judgment work, then recommends a plain script, a human-drawn graph, an agent loop, or a single model call with a person staying in charge, and says where the person sits. Use this whenever someone describes a business process or task and asks whether it is a good fit for AI, whether to build an agent, whether something simpler would do, whether a human should stay in the loop, how to structure or automate a workflow, or which of several processes to automate first. Trigger even when "automate" or "AI" never appears, for example "should we build an agent for this", "is this worth handing to AI", "do we need a human in the loop here", or a list of candidate processes with a request to prioritize them. Do not use for writing prompts, debugging agent code, or comparing specific models.
---

# AI Work Fit

Most disappointment with AI projects traces back to one mistake: reaching for the most impressive architecture instead of the right one. This skill asks two questions in order. What kind of work is this? Then, given that, what is the cheapest build that does the job, and who stays accountable for what?

Those are different questions, and people blur them. A lot of what looks like an AI problem is a software problem in costume, and a lot of what gets built as an autonomous agent would run just as well, and far more cheaply, as a fixed sequence of steps with a model plugged in only where real interpretation happens. The aim is not to be conservative with AI. It is to avoid paying for autonomy that buys nothing.

It is the operational version of two essays:
- [Not All AI Work Is AI Work](https://jiyad-ahsan.medium.com/not-all-ai-work-is-ai-work-23f2c9d80ce6)
- [Agents, Loops, and Graphs (and When to Use Neither)](https://medium.com/@jiyad-ahsan/agents-loops-and-graphs-and-when-to-use-neither-54fdfc71aa1d)

## The flow

1. Split the request into its separate decisions. A request phrased as one workflow is almost always a bundle, and classifying the bundle as one thing is the most common way to get this wrong.
2. Classify each decision (Step 1).
3. Match each to a build, a model tier, and a human role (Step 2).
4. Run the checks that can overrule the match (Step 3).
5. If there are several candidates, rank them (Step 4).
6. Report in the output format.

## Step 1: Classify the work

Walk these questions in order and stop at the first one that resolves the task.

**Is the mapping from input to output fixed?** Given the same inputs, is there one correct output every time, with no real interpretation? Then it is **routine work**. It was always a software problem, whatever it gets relabeled as this year. Go to Step 2: the answer is a script.

**If not, has the pattern become learnable?** The work involves real ambiguity, such as weighing competing signals or reading tone and context, but if you watched enough examples of the decision being made, would the pattern converge into something you could write down? Then it is **learnable work**. A quick test: could a capable new hire do it from a one-page checklist (routine), or only after watching a few dozen cases (learnable)? This type has a shelf life. Once the pattern is legible it should be demoted into a script, not kept running through a model forever.

**If not, is there an external check on the outcome?** The work is novel each time and no fixed route survives the next case. But is there something outside the decision itself that can say whether it was right: a test that passes or fails, a number that has to reconcile, an outcome that later confirms or contradicts it? Then it is **verifiable work**, provided the check is good enough to act on (Step 3). If the rightness of the answer is a stance someone takes and owns rather than a fact waiting to be found, it is **judgment work**.

Don't force a task into routine or learnable because it sounds simple to describe. "Screen resumes" sounds like one task but is usually two: the eligibility filter (routine) and the call on who gets a phone screen (learnable).

## Step 2: Match the build and the human role

Terms, used the same way throughout:
- **Script**: fixed code, no model.
- **Graph**: a human-drawn sequence of steps where a model runs only at the node or two that need interpretation. It is a workflow, not a graph framework and not a knowledge graph.
- **Loop**: an agent that acts, checks the result against a check, and tries again until the check passes or a budget runs out.
- **Single call**: one model call with the relevant context supplied and no iteration.

| Type | Build | Model tier | Who does what |
|---|---|---|---|
| Routine | Script. If the input is messy text, one model call at the parse step is fine, but the decision logic stays deterministic. | None, or the smallest model at the parse step | Machine end to end. A person owns the exceptions queue and spot-checks output. |
| Learnable | Graph, with the model at the interpretive step only. | The smallest model that matches a person's calls on a sample of real cases. This work behaves like a commodity, so frontier capability is wasted spend. | The model makes the routine calls. A person reviews a sample and takes low-confidence cases until the pattern is clear enough to become a script. |
| Verifiable | Loop, with the check wired in as the thing it iterates against. | The strongest model the budget allows. Every iteration is checked, so extra capability compounds into correct results. | The agent iterates. A person defines and owns the check, sets the budget, and receives the escalation when the check cannot be met. |
| Judgment | Single call. One good draft or one clear rundown of options. | One strong call. | A person decides and owns the outcome. The model prepares the draft or the options. |

Why these builds: a drawn route is cheaper and easier to debug than a model improvising it every time, which is why learnable work gets a graph. The check is what makes autonomy converge instead of wander, which is why a loop needs one. And with judgment there is no outcome to generate multiple drafts against, so more generation just adds a step before a person still has to decide.

If a task looks learnable but the organization does not yet know its own process well enough to draw the graph, an agent is a legitimate temporary scout for discovering the route. Say so, and say that the agent should be replaced by a fixed graph once the route is known.

If a step is physical or relational (a face-to-face conversation, a signature), it stays with a person. Classify only the decision part.

## Step 3: Checks that can overrule the match

**Is the check good enough to loop against?** A weak check turns a loop into an expensive way to be confidently wrong. Test it four ways:
- Independent: it does not depend on the agent's own judgment of its work.
- Fast and cheap enough to run on every iteration.
- Faithful: it measures the real goal, not a proxy the agent can satisfy without achieving the goal (tests that pass while the behavior is wrong).
- Conclusive within the time the loop runs, not months later.

If it fails, do not build the loop. The work is still verifiable in principle, but the build changes until a better check exists: build the check first, or use a graph or single call with a person as the check. A person can be the check, but they are the slowest and costliest one, so cap those rounds at two or three.

**Does the loop have guardrails?** Cap iterations and spend. Give it the least access that still lets it work. Define what it does when the check cannot pass (stop and escalate, don't keep trying). Log the attempts so a person can see what it did.

**Do the stakes change who signs off?** If an action is irreversible, regulated, or reaches outside the organization (moving money, messaging customers, changing production), a person approves the final action whatever the type. This changes who, not what type of work it is.

**Is it worth building at all?** If the task is rare, cheap, or already takes a person a few minutes, leaving it manual is a legitimate answer. Say so plainly rather than inventing an architecture for it.

**Has the type changed?** Learnable work graduates to routine once a written rule reproduces a person's calls on a sample of past cases. In the other direction, routine work whose inputs start to drift (new formats, new suppliers) has become learnable.

## Step 4: Ranking several candidates

Type tells you how cheap a build is. Volume tells you what it pays back. A routine task that runs twice a year is a worse first project than a learnable task that runs thousands of times, so don't rank on type alone. For each candidate note how often it runs, what each run costs now (person-minutes, or the cost of errors), and readiness (inputs digital, process documented, a check available). Rank on payoff against build effort. When payoff is similar, the order is routine, then learnable, then verifiable with a strong check, then judgment (assist only). State that the ranking is an estimate from what you were told, and name the one number that would change it.

## When the type depends on a missing fact

Sometimes the description alone cannot settle it. Most often the missing fact is whether a trustworthy check exists. Sometimes it is whether a rule could be written today, or how often the task runs. "Decide our budget split across channels" is verifiable if there is a measurable target and history to learn from, but judgment if it is an unproven bet with no ground truth yet.

Don't guess, and don't default to the more impressive answer because the person said "agent". Name the specific fact that decides it, then give the classification and build for each branch. Two clean conditional answers beat one confident wrong one, and surfacing the missing fact is often the most valuable thing this skill does, because it is usually the thing the person had not realized was load-bearing. If the fact is quick for them to supply, end by asking for just that one thing.

## Output format

For a single task:

```
Task: [one-line restatement]
Type: [Routine / Learnable / Verifiable / Judgment]
Why: [1-2 sentences citing which question resolved it]
Build: [Script / Graph / Loop (agent) / Single call / None, leave it manual]
Human role: [where the person sits and what they own]
Note: [shelf-life, scout phase, check quality, sign-off, or omit]
```

For several tasks, or one workflow that turns out to be a bundle, use a table with the columns Task, Type, Build, Human role, and add Priority when ranking. Put notes under the table. Without a ranking request, sort routine first and judgment last. When the type depends on a missing fact, add a "Deciding fact:" line and answer both branches.

## A worked example

"We want AI to handle customer support end to end."

This is several decisions. Routing a ticket by keyword is routine (script; the person owns the exceptions queue). Deciding whether an angry message needs escalation is learnable (graph, with a model doing the tone-reading step; a person reviews a sample). Diagnosing why one customer's integration keeps failing is verifiable (loop, an agent trying fixes against the actual error until one resolves it; a person owns the check and gets the escalation if it never resolves). Deciding whether to comp a furious customer's subscription to protect the relationship is judgment (single call; the model drafts, the person who owns that relationship decides).

More worked examples, including a prioritization, a "don't automate this" answer, a scout agent, and a loop with a weak check, are in `references/examples.md`. Read it when the domain is unfamiliar or the request is one of those shapes.
