# Token-saving levers

In approximate order of impact.

1. **Fewer agents, better grouped.** Each new agent has a fixed cold-start cost.
2. **The right model per task.** See [model-triage.md](model-triage.md).
3. **Minimal context package.** Exact paths and excerpts, never "read the repo".
4. **Short reports in a fixed format.** See [agent-prompt.md](agent-prompt.md).
5. **Always filter command output.** Never dump a full log into the context.
   ```bash
   <command> 2>&1 | grep -E 'ERROR|FAIL|BUILD' | tail -40
   ```
   Adapt the filter to the project's tooling.
6. **WIP commit before resuming.** The new agent reads `git log` and the diff instead of rebuilding context.
7. **A batch of small fixes goes to a single cheap agent**, not several.
8. **Verification by the orchestrator with a command**, not by an extra agent.
9. **Lean log.** Only decisions that change behavior. See [decision-log.md](decision-log.md).
10. **Discover the repo's verification command** (README, `package.json`, `pom.xml`, Makefile, CI) instead of exploring to find it.
11. **The reviewer receives the diff and the criteria**, not the repository.
12. **Calibration.** Use the `estimated vs actual` from the log to estimate better on future tasks. See [budget.md](budget.md).
