# Entry gate

First step, always. The answer is **one line**: is orchestration worth it?

Orchestration has overhead (cold starts, reports, integration). It is only worth it if **at least one** of these is true:

- There are 2 or more independent units of work, including different contexts of the same project (e.g., fixing finance and registration in the same request).
- The work does not fit in a single context.
- Independent review is required (the reviewer cannot be the one who wrote the code).

## Decision

| Situation | Action |
| --- | --- |
| No condition is true | The orchestrator does it alone |
| One unit, but large or requiring review | A single agent |
| Two or more independent units | Orchestrate |
| Units that share files | Serialize, do not parallelize |

Do not spend more than one or two lines justifying the gate. Record the result in the log (`gate` field).
