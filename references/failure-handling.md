# Failure handling

## General rule

**At most 1 retry with the same approach.** After 2 failures, the orchestrator stops: it resolves the issue itself if it is small, or asks the user.

Never repeat the same thing. Change the model, scope or prompt.

## Scenarios

| Scenario | Detection | Action |
| --- | --- | --- |
| Session or weekly limit | Environment error | WIP commit, stop, record state in the log, resume later with a new agent |
| Agent stuck or unresponsive | No progress or timeout | Terminate, check the repo, relaunch with a smaller scope |
| Said "done" but verification fails | Orchestrator runs the command | Return only the filtered error, once |
| Changed files outside scope | `git diff --name-only` | Revert the excess, relaunch with an explicit prohibition |
| Reported without evidence or hallucinated | Report has no command result | Ignore the report, verify in the repo |
| Loop on the same error | Same error twice | Change model, scope or prompt |
| Context overflow | Agent losing the thread | Split the task, new agent starting from the committed WIP |
| Machine slept or interruption | Inconsistent repo state | Check `git status` and `git log` before resuming |
| Conflicting agent results | Merge or verification breaks | See [merge-and-review.md](merge-and-review.md) |
| Ambiguous requirement or blocker | Agent reports `blocked` | Make the most conservative decision and record it; if irreversible, ask the user |
| Rate limit | API limit error | Reduce concurrency, record it, resume cheaper |

## Resuming

Default: **new agent with a short prompt**, which reads the `git log` and the WIP diff. Resuming an old agent reloads its entire transcript.

Exception: if the old agent was almost done with a complex task and the context is hard to reconstruct, resuming may cost less. Evaluate case by case.

Before resuming: check the actual state of the repo. Do not trust your memory of what "should" be there.

## Recording

Every relevant failure goes into the **Incidents** table of the log. See [decision-log.md](decision-log.md).
