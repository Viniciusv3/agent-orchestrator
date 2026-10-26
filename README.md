# agent-orchestrator

A skill for orchestrating multiple agents on development tasks with **the lowest possible token spend**, without losing quality.

It was born from lessons learned in a real orchestration (front end and back end of a SaaS, several repositories, several agents) and generalized: nothing here depends on language, framework or project.

## What it does

- **Entry gate:** decides whether orchestration is worth it or whether it is better to do the work alone.
- **Model triage:** picks the cheapest model that can handle each task, based on 5 factors, with no fixed table.
- **P/M/G budget:** sizes tasks, reserves 25 to 30% for retries, and calibrates using `estimated vs actual`.
- **Adaptive concurrency:** no fixed number of agents; backs off when it hits a limit.
- **"Done" verified by the orchestrator:** command, scope and placeholders, without trusting the agent's report.
- **Safe merging:** overlap prevention, detection before merging, tiered resolution.
- **Failure handling:** a table of scenarios and a rule of at most 1 identical retry.
- **Decision log:** `.orchestration/log.md` with a fixed schema, ready to feed a dashboard.
- **Push only on explicit request** from the user, even with bypass permissions.

## Structure

```
agent-orchestrator/
├── SKILL.md                      # flow and rules that always apply (always loaded)
└── references/                   # loaded only at the step where they are cited
    ├── entry-gate.md
    ├── planning.md
    ├── model-triage.md
    ├── budget.md
    ├── agent-prompt.md
    ├── definition-of-done.md
    ├── merge-and-review.md
    ├── failure-handling.md
    ├── decision-log.md
    ├── token-savings.md
    └── safety.md
```

`SKILL.md` is deliberately lean: the agent reads each reference only when it reaches the corresponding step, which saves tokens.

## Installation

Copy the `agent-orchestrator/` folder into your environment's skills directory and invoke it with `/agent-orchestrator`.

## Status

v1. The P/M/G sizes and the budget reserve are starting points; adjust them based on the `estimated vs actual` recorded in the log.
