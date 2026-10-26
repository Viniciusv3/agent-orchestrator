---
name: agent-orchestrator
description: Orchestrates multiple agents (sub-agents) on development tasks with the lowest possible token spend, without losing quality. Use when the request involves multiple modules, repositories or distinct contexts, parallel work or independent review. Handles model triage, per-agent budgeting, verification, safe merging, failure handling and decision logging. Do not use for small tasks that fit in a single context.
---

# Agent Orchestrator

## Top priority

**Token savings, reaching the same result.** When choosing between two options that deliver the same result, pick the cheaper one.

- Parallelism saves time, not tokens. Every new agent pays a cold start (re-reading context).
- The orchestrator does the mechanical work: commit, merge, run checks, read diffs, clean up worktrees. Agents are only for work that requires reading and writing code.
- Read each file in `references/` **only at the step where it is cited**. Do not load them all at once.

## Flow

1. **Entry gate.** Is orchestration worth it? Answer in one line. If not, do it yourself or use a single agent. → [references/entry-gate.md](references/entry-gate.md)
2. **Planning.** Read the minimum, split into units, check for file overlap, define order. → [references/planning.md](references/planning.md)
3. **Model triage** for each agent, before spawning. → [references/model-triage.md](references/model-triage.md)
4. **Budget** (P/M/G size, reserve, adaptive concurrency). → [references/budget.md](references/budget.md)
5. **Agent prompt** short, with criteria and a report in a fixed format. → [references/agent-prompt.md](references/agent-prompt.md)
6. **"Done" check** performed by the orchestrator, never by the agent's "passed". → [references/definition-of-done.md](references/definition-of-done.md)
7. **Merge, conflicts, integrator and reviewer.** → [references/merge-and-review.md](references/merge-and-review.md)
8. **Failures** when something goes wrong. → [references/failure-handling.md](references/failure-handling.md)
9. **Decision log** throughout the whole run. → [references/decision-log.md](references/decision-log.md)
10. **Wrap-up.** → section below.

Cross-cutting, consult when relevant:
- Token-saving levers → [references/token-savings.md](references/token-savings.md)
- Safety and push policy → [references/safety.md](references/safety.md)

## Rules that always apply

- Every agent is born with an explicit `model`. Without it, it inherits the most expensive model in the session.
- A size-G task does not go to a single agent: split it.
- No push, PR or publication without **explicit request from the user in the conversation**, even with bypass permissions.
- At most 1 retry with the same approach. On the second failure, change model, scope or prompt.
- The orchestrator runs the verification itself, after each merge.
- The reviewer is never the one who wrote the code.

## Wrap-up

1. Merges done and final verification green, run by the orchestrator.
2. Agent worktrees and branches removed; `git status` clean.
3. Pending items recorded in the repo documentation, not only in the log.
4. Summary to the user in a few lines: done, pending, estimated vs actual spend.

## Quick checklist

- [ ] Entry gate answered
- [ ] Minimum context read
- [ ] Units split, no file overlap, G tasks broken down
- [ ] Explicit model per triage on every spawn
- [ ] Concurrency starting low
- [ ] Short prompt with scope, criteria, verification command and report
- [ ] Verification run by the orchestrator after each merge
- [ ] Integrator (if needed) and then an independent reviewer
- [ ] No push without explicit request
- [ ] Log updated, worktrees clean, pending items in `docs/`
