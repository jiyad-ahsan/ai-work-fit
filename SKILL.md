---
name: ai-work-fit
description: Determines whether a described task or workflow is routine, learnable, verifiable, or judgment work, then recommends the right way to build it, a plain script, a human-drawn graph, an autonomous agent loop, or a single model call with a human staying in charge. Use this whenever someone describes a business process or task and asks whether it's a good fit for AI, whether to build an agent, whether it needs a full agent loop or something simpler, how to automate a workflow, or how to decide between a chatbot, a pipeline, and a script. Trigger even without the words "automate" or "AI" in the request, phrases like "should we build an agent for this", "is this worth handing to AI", "how should this workflow be structured", "do we need a human in the loop here", or someone listing several candidate processes and asking which to prioritize for automation should all trigger this skill.
---

# AI Work Fit

Most disappointment with AI projects traces back to one mistake: reaching for the most impressive architecture instead of the right one. This skill runs a task through two short diagnostics, first sorting what kind of work it actually is, then matching that to the cheapest architecture that will actually do the job.

It's the operational version of two essays:
- [Not All AI Work Is AI Work](https://jiyad-ahsan.medium.com/not-all-ai-work-is-ai-work-23f2c9d80ce6)
- [Agents, Loops, and Graphs (and When to Use Neither)](https://medium.com/@jiyad-ahsan/agents-loops-and-graphs-and-when-to-use-neither-54fdfc71aa1d)

## Why the two-step split matters

People conflate two different questions: "is this worth handing to AI" and "should this be an agent." They aren't the same question. A lot of what looks like an AI problem is actually a software problem wearing an AI costume, and a lot of what gets built as an autonomous agent would work just as well, and far more cheaply, as a fixed sequence of steps with a model plugged in only where real judgment is needed. Getting this right isn't about being conservative with AI, it's about not paying for autonomy that isn't buying anything.

## Step 1: Classify the work

Walk through these questions in order. Stop at the first one that resolves the task, later questions only matter for work that survives the earlier ones.

**Is the mapping from input to output fixed?** Given the same inputs, is there one correct output every time, with no real interpretation involved? If yes, this is **routine work**. It was always a software problem, no matter what it gets relabeled as this year. Go straight to the architecture step, the answer is a script.

**If not, has the pattern become learnable?** The work involves real ambiguity, weighing competing signals, reading tone or context, but if you watched enough examples of this decision get made, would the pattern converge into something you could write down as a rule? If yes, this is **learnable work**. Keep in mind this type has a shelf life: once the pattern is legible, it should get demoted into a script, not kept running through a model forever.

**If not, is there an external check on the outcome?** The work is genuinely novel each time, no fixed route survives contact with the next case, but is there something outside the decision itself that can tell you whether it was right, a test that passes or fails, a number that has to reconcile, an outcome that eventually confirms or contradicts it? If yes, this is **verifiable work**. If no, if the "rightness" of the answer is a stance someone takes and owns rather than a fact waiting to be discovered, this is **judgment work**.

Don't force a task into routine or learnable just because it sounds simple to describe. "Screen resumes" sounds like one task but is usually two: the keyword filter (routine) and the actual judgment call about who gets a phone screen (learnable). Split compound workflows into their component decisions before classifying any of them.

## Step 2: Match the architecture

| Type | Build | Why |
|---|---|---|
| Routine | A script. No model, no agent. | There's nothing to decide, so a model adds nothing but cost and latency. |
| Learnable | A graph: a human-drawn sequence of steps, with a model filling in only the step or two where real interpretation happens. | A human can draw the route because the pattern is knowable, and a drawn route is cheaper and easier to debug than a model improvising it every time. |
| Verifiable | A loop, an agent, with the check wired in as the thing it iterates against. | The check is what makes autonomy converge instead of wander. Without something to check against, a loop just spins. |
| Judgment | A single call. One good draft or one clear rundown of the options, handed to the accountable person. | There's no outcome to generate multiple drafts against, so more generation doesn't save anything, it just adds a step before a human still has to decide. |

One case worth flagging explicitly when it applies: if a task looks like learnable work but the organization doesn't yet know its own process well enough to draw the graph, an agent is a legitimate *temporary* scout, useful for discovering the route, not for running it forever. Say so, and note that the agent should get replaced by a fixed graph once the route is known.

## When the type depends on a missing fact

Sometimes a task can't be classified from the description alone because it hinges on something the person hasn't said, most often whether an external check exists. "Decide our budget split across channels" is verifiable work if there's a measurable target to optimize against and history to learn from, but judgment work if it's an unproven strategic bet with no ground truth yet.

When this happens, don't guess, and don't default to the more impressive answer just because the person used a word like "agent." Name the specific fact that decides it, then give the classification and build for each branch. Two clean conditional answers are far more useful than one confident wrong one, and surfacing the missing fact is often the most valuable thing this skill does, because that fact is usually the thing the person hadn't realized was load-bearing.

## Output format

For a single task, respond with:

```
Task: [one-line restatement]
Type: [Routine / Learnable / Verifiable / Judgment]
Why: [1-2 sentences citing which diagnostic question resolved it]
Build: [Script / Graph / Loop (agent) / Single call]
Note: [shelf-life warning, scout-phase note, or omit if nothing applies]
```

For multiple tasks or a single workflow that turns out to be a bundle of several decisions, use a table with those same columns instead. Sort routine work first (cheapest to fix) and judgment work last.

## A worked example, for calibration

"We want AI to handle customer support end to end."

Don't classify this as one thing, it's several. Routing a ticket by keyword is routine (script). Deciding whether an angry message needs escalation is learnable (graph, with a model doing the tone-reading step). Diagnosing why one customer's integration keeps failing is verifiable (loop, an agent trying fixes against the actual error until one resolves it). Deciding whether to comp a furious customer's subscription to protect the relationship is judgment (single call, a recommendation handed to the person who owns that call). A request phrased as one workflow is almost always a bundle. Always split it before classifying.
