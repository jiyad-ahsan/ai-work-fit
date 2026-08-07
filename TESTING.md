# Testing

How this skill was validated, and what correct behavior looks like. If you modify the skill, re-run these three cases, they cover the three behaviors that matter most.

## Test 1: Compound workflow splitting

**Input:** "We want to automate our entire vendor onboarding process."

**Expected behavior:** The skill must not classify the request as one type. It should split the workflow into component decisions and classify each, producing a table spanning multiple types. A reasonable output:

| Task | Type | Build |
|---|---|---|
| Collect standard onboarding documents (W-9, insurance cert, bank details) | Routine | Script |
| Assess a new vendor's risk profile and set initial credit terms | Learnable | Graph |
| Investigate a flagged compliance mismatch against a business registry | Verifiable | Loop (agent) |
| Onboard a strategically important but higher-risk vendor outside normal policy | Judgment | Single call |

**Failure mode this catches:** classifying a bundle as a single type, usually whichever type the most prominent-sounding sub-task suggests.

## Test 2: Missing-fact ambiguity

**Input:** "We're building an agent to decide our marketing budget split across channels next quarter."

**Expected behavior:** The skill should refuse to pick a single type, because the classification hinges on a fact the request doesn't contain: whether a measurable target (ROAS/CAC goal, conversion number) and channel history exist to optimize against. It should name that fact explicitly and answer both branches, verifiable → loop if the check exists, judgment → single call if it doesn't.

**Failure mode this catches:** defaulting to "verifiable, build the agent" because the user said the word "agent." The skill exists precisely to resist that bias.

## Test 3: Clean routine work

**Input:** "Extract the tracking number from carrier shipping-confirmation emails and log it against the order."

**Expected behavior:** Routine, resolved at the first diagnostic question, build a script. Ideally it also notes the acceptable nuance: if emails are messy across carriers, a single model call may do the extraction more robustly than regex, but that's still a call inside a script, never an agent.

**Failure mode this catches:** type inflation, dressing up a fixed input-output mapping as AI work.
