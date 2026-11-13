# Progress across sessions

Use this record for a broad cleanup or work likely to span sessions. Read an existing record even during an audit, but write it only when cleanup or saving findings is authorized. Prefer the project's existing debt tracker; do not introduce a second source of truth.

When no tracker exists, use `docs/implementation/codebase-cleanup.md` if it fits the repository's documentation layout, otherwise `CODEBASE-CLEANUP.md`. Follow repository language and file conventions. Create neither a new tool-specific state directory nor an unrelated directory reorganization.

## Record the useful state

Keep the record concise enough to resume from:

```markdown
# Codebase cleanup

## Scope and baseline

- Scope and exclusions:
- Intended behavior and important contracts:
- Last assessed revision and date:
- Relevant uncommitted changes at that assessment:
- Baseline checks, results, and pre-existing failures:
- Coverage: areas inspected and areas not yet assessed:

## Findings

| ID | Priority | Status | Evidence and consequence | Remedy and verification | Remaining uncertainty or deferral reason |
| --- | --- | --- | --- | --- | --- |

## Last completed batch

- Findings addressed and changed paths:
- Verified revision, or revision plus a summary of the uncommitted batch:
- Commands, results, and relevant environment limits:

## Next work

- Next coherent batch and why:
- Unassessed areas and remaining checks:
- Blockers requiring user input or outside evidence:
```

Use stable finding IDs within this record and these statuses:

- **queued:** confirmed actionable work, not started.
- **active:** changed or being investigated; verification is incomplete.
- **verified:** remedy and required checks confirmed on the recorded tree.
- **deferred:** intentionally left in place with a concrete reason and condition for reconsideration.
- **blocked:** cannot proceed without a named missing input or external condition.

Discard a disproven finding with an explanation rather than counting it as a fix. Summarize recurring findings under the shared cause, retaining IDs or references so previous decisions remain traceable. Do not paste credentials, private content, raw tool transcripts, or full patches into the record.

## Resume procedure

Compare the recorded revision and uncommitted-batch summary with current Git state. Re-read the affected symbols and consumers; evidence and line numbers may be stale. Confirm whether previous checks still cover the current code and dependencies. Findings fixed elsewhere can be closed only with current evidence; changed assumptions reopen them.

Update the record after a verified batch and before handing work back. A checklist tick or a historical pass is not proof that a current change is safe. Deferral records unfinished work; it does not make that work complete. State remaining scope and checks explicitly in the final response.
