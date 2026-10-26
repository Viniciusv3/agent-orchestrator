# Decision log

A single file: `.orchestration/log.md`. **Append-only, written only by the orchestrator.** Agents send "decisions" in their report, and the orchestrator copies what matters.

The schema is fixed and machine-readable, so a future dashboard can read the file without rework.

## Schema

````markdown
# Orchestration: <short task name>

## Run
```yaml
start: <ISO 8601>
goal: <1 line>
gate: <orchestrate | alone | single agent> — <short reason>
global_budget: <units>
reserve: <units>
```

## Agents
```yaml
- id: a1
  role: <role>
  model: <model>
  expected_size: P|M|G
  branch: feat/<role>
  scope: [<folders/files>]
  status: pending|running|done|partial|blocked|failed
  estimated_vs_actual: <e.g.: M / diff 180 lines, 1 retry>
  retries: 0
```

## Decisions
| date | decision | reason | discarded alternative | who |
| --- | --- | --- | --- | --- |

## Incidents
| date | event | action |
| --- | --- | --- |
````

## What to record

Only what changes the behavior of others or deviates from the spec:

- model outside the triage standard
- conservative decision due to ambiguity
- scope change
- resolved conflict
- rate limit and concurrency reduction
- failures and how they were handled

**This is not a step-by-step diary.** Otherwise it becomes a reading cost. Agents read only what is relevant to them.

## Resume point

The **Agents** section serves as state for resuming after a session limit. Update the `status` on every relevant change.

## At the end

Whatever is lasting moves into the repo's `docs/`. The log can be discarded or archived. It is listed in `.gitignore` by default.
