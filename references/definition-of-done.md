# Definition of "done"

Verified by the orchestrator with inexpensive commands. Without spending an agent and without trusting the agent's "passed".

## Done = all true

- [ ] **Observable acceptance criteria** met. Example: "screen X reads from the real API, not from the mock".
- [ ] **Verification command run by the orchestrator itself** and green.
- [ ] **Scope respected:** `git diff --name-only` contains only allowed files.
- [ ] **No placeholders** in the diff: search for `TODO`, `FIXME` and stubs.
- [ ] **Report in the fixed format**, with a command result.

## How to write good acceptance criteria

Observable and checkable, not adjectives.

| Bad | Good |
| --- | --- |
| "Integration working" | "Screen X calls endpoint Y, with no mocked data on the path" |
| "Clean code" | "No import of `module-b` inside `module-a`" |
| "Error handling" | "A network failure shows a message and does not break the page; a test covers it" |

## Inexpensive verification commands

```bash
git diff --name-only <base>...HEAD                     # scope
git diff <base>...HEAD | grep -nE 'TODO|FIXME'         # placeholders
<verification command> 2>&1 | grep -E 'ERROR|FAIL' | tail -40
```

## If it is not done

Follow [failure-handling.md](failure-handling.md): return only the filtered error, once.
