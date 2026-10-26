# Budget and concurrency

The budget is **task sizing**, not a hard limit. There is no automatic token cutoff, and the agent cannot measure its own consumption precisely. The limit works through the proxies the orchestrator controls.

## Sizes

Use ranges, never an exact token count. Estimating exact tokens burns expensive reasoning on an estimate that will be wrong.

| Size | Typical scope | Context read |
| --- | --- | --- |
| P | 1 to 3 files, existing pattern | only the files cited |
| M | 1 module, a few files | the module's context |
| G | crosses modules or contracts | requires a system map |

**A G task does not go to one agent. Split it into P and M.**

## How the limit is applied

- **At intake:** only assign work that fits the range. If it doesn't fit, split it.
- **In the prompt:** state the range and the stop rule: "if you estimate you will exceed the budget, or make the same mistake twice, make a WIP commit, stop and report `partial` with what is missing". The practical signals are the number of files read and the number of trial-and-error cycles.
- **On output:** compare the expected range with the actual outcome (diff size, retries) and record it in the log.

## Global budget

Reserve **25 to 30%** for retries, integration and review. Without a reserve, the first retry blows the entire budget.

## Cost weighted by model

cost = size × model weight. An M on a cheap model costs much less than an M on an expensive model. Use this to decide between splitting further or moving up a model.

## Calibration

Record `estimated vs actual` per task. If a task type always overruns, estimate higher next time.

An overly tight budget makes agents stop early, and you end up paying more in resumptions. Adjust it.

## Adaptive concurrency

There is no fixed number of simultaneous agents.

- Start low (1 or 2 in parallel). Increase only if there are truly independent units.
- If you hit a rate limit, or a session or weekly limit: reduce concurrency, record it in the log and resume with a new, cheaper agent.
- Parallelize only when there is a time gain or context isolation.
