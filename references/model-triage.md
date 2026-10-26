# Model triage

Do this before **each** spawn. Every agent is born with an explicit `model`.

Triage is short. The orchestrator is usually the most expensive model in the session and should not spend long reasoning to make this decision.

## The 5 factors

| Factor | Pulls toward a cheaper model | Pulls toward a stronger model |
| --- | --- | --- |
| Verifiability | A command says whether it is correct (it compiles, the test passes) | Only human or another agent's review would notice the error |
| Ambiguity | An existing pattern to follow | An open design decision |
| Blast radius | One file or module | Crosses modules or contracts |
| Context needed | Small and localized | Needs to understand the whole system |
| Cost of error | Cheap to redo, reversible | Silent error, hard to detect |

## Rule: expected cost

A cheap model plus a retry usually wins when the failure shows up on its own, that is, when the task is verifiable by machine. When the error is silent, the cheap option ends up costing more.

Decide using the 5 factors. Do not use a fixed table of role-to-model.

## Safety net

There is no fixed escalation ladder. If the agent fails with evidence (the same error twice, review rejects it), change something before retrying: model, scope or prompt.

Move up a model only if the diagnosis points to a lack of capability. If the problem is a poor prompt or a large scope, moving up a model only spends more.

## Mechanical tasks with a cheap model

When the cheap agent is going to adjust code guided by a compiler error or an existing pattern, include in the prompt: **do not change the signature of a shared interface or domain logic just to make it compile**. The orchestrator checks the diff.

## Reviewer

Choose the reviewer's model using the same triage. It is usually stronger than the executor's, because the error it looks for is the silent one.
