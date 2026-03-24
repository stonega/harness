# AGENTS.md

This document defines how AI agents should work in this repository.

Agents must follow these conventions when reading, modifying, or generating code.

---

# 1. Project Structure

Repository layout:

project/
├ README.md
├ AGENTS.md
├ docs/
│   ├ design/          # architecture and system design
│   ├ implementation/  # technical implementation notes
│   ├ research/        # experiments and investigations
│   ├ reference/       # API references and external resources
│   └ user/            # end-user documentation
│
├ src/                 # main source code
├ tests/               # automated tests
├ scripts/             # automation scripts
├ postmortem/          # incident reports and retrospectives
└ examples/            # usage examples

Agents must preserve this structure.

---

# 2. Development Workflow

Agents must follow this workflow when implementing features.

1. Read relevant documents in:

docs/design
docs/implementation

2. Implement code inside:

src/

3. Add or update tests inside:

tests/

4. Update documentation when necessary.

---

# 3. Coding Rules

Agents must follow these rules:

- Do not introduce new frameworks without justification
- Keep functions small and composable
- Prefer readability over cleverness
- Avoid duplication

When modifying code:

- Prefer editing existing modules over creating new ones
- Follow existing naming conventions

---

# 4. Testing Requirements

All new features must include tests.

Tests must be placed in:

tests/

Test guidelines:

- Prefer unit tests
- Add integration tests for workflows
- Tests must be deterministic
- Avoid external dependencies

Agents must run tests before finishing a task.

---

# 5. Documentation Rules

Documentation must be updated when:

- architecture changes
- APIs change
- new workflows are added

Docs location rules:

Architecture → docs/design  
Implementation detail → docs/implementation  
User documentation → docs/user  

---

# 6. Examples

When introducing new APIs or workflows, agents must add examples in:

examples/

Examples must be runnable.

---

# 7. Scripts

Automation scripts go into:

scripts/

Examples:

build
deploy
test
data migration

Scripts should be idempotent.

---

# 8. Postmortems

When bugs or incidents occur, create a report in:

postmortem/

Postmortem format:

incident.md

Include:

- what happened
- root cause
- fix
- prevention

Agents may reference postmortems to avoid repeating past mistakes.

---

# 9. Safety Rules

Agents must NOT:

- delete large sections of code without reason
- change project structure
- modify dependencies without explanation

When unsure, agents should request clarification.

---

# 10. Pull Requests

Agent-generated changes must:

- pass tests
- follow coding conventions
- include documentation if required
