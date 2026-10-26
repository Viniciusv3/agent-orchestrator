# Agent prompt

Keep it short. Each prompt includes:

1. **Role and scope:** what it owns, which files it may touch, what it must not touch.
2. **Minimum context:** exact file paths and excerpts to read. "Read only what is necessary." Never "explore the repo".
3. **Size (P/M/G) and stop rule.** See [budget.md](budget.md).
4. **Observable acceptance criteria.** See [definition-of-done.md](definition-of-done.md).
5. **Verification command** that must pass before delivery, with filtered output.
6. **Report format** (below), short, without repeating code.
7. **Commits:** WIP checkpoint before any pause, so work can be resumed.
8. **Prohibitions:** no push, no real secrets, no changes outside scope, no changes to public contracts or signatures beyond what was requested.

## Prompt template

```
Role: <role>. You own: <folders/files>. Do not touch: <everything else>.
Context (read only what is necessary): <paths>.
Task: <1 to 3 lines>.
Size: <P|M>. If you estimate you will exceed the budget or make the same mistake twice:
commit WIP, stop and report "partial".
Acceptance: <observable criteria>.
Verification: <command> (filter the output, show only errors).
Commits: WIP before any pause.
Forbidden: push, real secrets, files outside scope.
Report (max 8 lines):
status: done | partial | blocked
files: ...
verification: <command + summarized result>
decisions: ...
pending: ...
```

## Report format

```
status: done | partial | blocked
files: <short list>
verification: <command + summarized result>
decisions: <one line each, only what deviates from the spec>
pending: <what is missing, if partial or blocked>
```

A report without a command result is treated as **unverified**.
