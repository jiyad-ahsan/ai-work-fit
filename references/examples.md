# Worked examples

Read this when the domain is unfamiliar, or when the request is a prioritization, a case where the right answer is "don't automate", a scout agent, or a loop with a weak check.

## Contents
1. Month-end close (a bundle with all four types)
2. Restaurant operations (and a prioritization)
3. Hiring (one workflow, two decisions hiding in one)
4. A loop with a weak check
5. Not worth automating
6. A scout agent for an undocumented process
7. Stakes change who signs off

## 1. Month-end close

"Can an agent do our month-end close?"

| Task | Type | Build | Human role |
|---|---|---|---|
| Match bank feed lines to ledger entries with exact amounts and references | Routine | Script | Owns the exceptions queue |
| Categorize ambiguous transactions (a vendor that sells several things) | Learnable | Graph, model at the categorize step | Reviews a sample; corrections feed the rules |
| Find why the bank balance and ledger still differ by an unexplained amount | Verifiable | Loop | Owns the check (the two balances reconcile) and the budget |
| Decide whether a disputed invoice is accrued, written off, or contested | Judgment | Single call | Decides; the model lays out the options and precedent |

Note: the categorizing step should be demoted to a script for any vendor whose category has not varied in a few dozen transactions.

## 2. Restaurant operations, and a prioritization

"Which of these should we automate first: reorder alerts, next-week prep forecast, food cost spike investigation, deciding whether to open a second branch, monthly supplier price report?"

| Task | Type | Build | Human role | Priority |
|---|---|---|---|---|
| Reorder alert when stock falls below par | Routine | Script | Reviews exceptions | 1 |
| Monthly supplier price report | Routine | Script | Reads it | 2 |
| Next-week prep forecast | Learnable | Graph, model at the demand-reading step | Adjusts for events the data can't see | 3 |
| Why did food cost jump this month | Verifiable | Loop | Owns the check (usage must reconcile with purchases) | 4 |
| Open a second branch | Judgment | Single call | Decides | Not a build |

Note: the ranking assumes the reorder alert runs daily and the price report monthly. If the price report is the one the owner spends an afternoon on, move it to 1. That number would change the order.

## 3. Hiring

"We want AI to screen resumes."

"Screen" hides two decisions. Rejecting applicants who lack a required license or work authorization is routine (script). Choosing who gets a phone screen from the rest is learnable (graph, model at the ranking step), and because it affects people, a person reviews every rejection from that step, not a sample. Deciding who gets the offer is judgment (single call; the model summarizes, the hiring manager decides).

## 4. A loop with a weak check

"Build an agent to keep improving our landing page conversion."

The tempting read is verifiable: conversion is a number. But the check fails three of the four tests. It is noisy and slow (days of traffic per variant), so it is not fast or conclusive. And it is gameable: a page can raise sign-ups by hiding the price.

Do not build the loop. Build a graph: a model proposes variants, a fixed experiment harness runs them with a stopping rule, and a person approves what ships. If the team later builds a fast, trustworthy proxy (for example a validated pre-test score), revisit.

## 5. Not worth automating

"Should we build an agent for our quarterly board-deck formatting? It takes about twenty minutes."

Leave it manual, or use a saved template. The task is rare and cheap, so no architecture pays back. Say so plainly.

## 6. A scout agent for an undocumented process

"Returns get approved in a dozen different ways depending on who's on shift. Can AI just figure out the policy?"

The work is probably learnable, but the organization cannot draw the graph because the route is not written down anywhere. An agent is a legitimate temporary scout: let it read a few hundred past returns and propose the implicit rules, then have the team confirm them. Replace it with a fixed graph (or a script, if the rules turn out to be crisp) as soon as the route is known. Do not keep the scout running forever.

## 7. Stakes change who signs off

"Let an agent pay supplier invoices that match the purchase order."

The match is verifiable (the invoice reconciles to the PO and the goods receipt), so a loop or even a script is right for the matching. But paying is irreversible and reaches outside the company, so a person approves the payment run, or approves any invoice above a limit they set. This changes who signs off, not the type of work.
