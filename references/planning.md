# Planning

## 1. Read the minimum

Product decision document, ownership map (e.g., `docs/ARCHITECTURE.md`) and contracts between layers (e.g., an `openapi.yaml`), **if they exist**. Do not read the entire repository.

Project-specific rules (architecture, encryption, conventions, design system) live in the repo's own documentation. The skill tells you to read them; it does not define them.

## 2. Split into units of work

- Group small, related tasks into the same agent: each new agent has a fixed starting cost.
- Separate into different agents only when the contexts are truly distinct.
- A task that crosses modules or contracts (size G) must be broken down. See [budget.md](budget.md).

## 3. Declare per agent

Role, allowed files and folders, acceptance criteria, verification command, size (P/M/G) and model.

## 4. Check for overlap before launching

Compare the allowed files of all agents. If two touch the same file: serialize them or define a single owner.

## 5. Check owners

Responsibility without an owner gets lost between agents. For each part of the request, confirm which agent owns it. If there is a gap, assign it before launching.

## 6. Define the order

- Foundation first, dependents afterward.
- Parallelize only what is independent: disjoint files and no dependency on outputs.
- With no time gain or context isolation, go sequentially.
- A circular dependency between modules is resolved with an event or a contract, never with a cross-import.
- Discover each repo's verification command (README, `package.json`, `pom.xml`, Makefile, CI) instead of assuming it.

## 7. Isolation

- One repository per layer, when there is more than one.
- Each agent works in `.worktrees/<repo>-<role>`, on the branch `feat/<role>`.
- Make sure `.worktrees/` and `.orchestration/` are in `.gitignore`.
