# Merge, conflicts, integrator and reviewer

The orchestrator does the merges. At the end it removes worktrees and branches and confirms the repo is clean.

## Prevention

This is the cheapest approach: declare the files per agent and check for overlap before spawning. See [planning.md](planning.md).

## Merge order

Foundation first, dependents afterward. **Run verification after each merge**, not just at the end. A change to a shared interface only shows up at merge time; finding everything at once generates large batches of errors.

## Detection before merging

```bash
git merge-tree --write-tree <main> <branch>     # shows conflicts without changing anything
# or
git merge --no-commit --no-ff <branch>          # simulates; then: git merge --abort
```

## Tiered resolution

| Level | Examples | Who resolves |
| --- | --- | --- |
| Trivial | imports, formatting, lockfile, generated file | The orchestrator. Lockfiles and generated files are **regenerated**, never merged by hand |
| Semantic | same function, same signature | Do not guess. Send only the conflicting excerpts and both diffs to a cheap agent, or return it to the module owner |

Never "accept one side" without understanding it. Always run verification afterward.

## Integrator

If several agents changed shared interfaces and the merge broke the build, launch an integrator agent on a cheap model, guided by the compiler error.

The integrator's prompt includes the prohibitions from [model-triage.md](model-triage.md): it only adapts what the error points to, and does not change signatures or domain logic just to make it compile. The orchestrator checks the diff.

## Reviewer

- Runs **last**.
- **Cannot be the one who wrote the code.**
- Points out problems. It may make small fixes; structural fixes go back to the module owner.
- Model chosen through triage, usually stronger than the executor's.
- Give the reviewer the diff and the acceptance criteria, not the entire repository.
