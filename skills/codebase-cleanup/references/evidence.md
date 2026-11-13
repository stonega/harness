# Evidence checklist

Use only the categories relevant to the requested scope. A finding needs a consequence and a verifiable remedy; this is not a quota of smells to report.

## Dead code and abandoned paths

Check imports and callers together with package exports, CLI and application entry points, route tables, plugin registries, string keys, reflection, scheduled jobs, build scripts, and import-time side effects. Trace production composition as well as tests. An exported library symbol can have consumers outside the repository; an old flag can still be used in deployed configuration or persisted records.

Searches returning no hits must have covered the relevant paths and ignored-file rules. Distinguish unreachable code, unused public surface, deliberately dormant behavior, and a path whose consumers are unknown. Delete only the proven case; do not erase a contract because the local test suite does not exercise it.

## Duplication and competing implementations

Compare inputs, outputs, failure semantics, permissions, lifecycle, and likely reasons to change. Repeated syntax can express distinct policies. Real debt includes duplicate parsers that disagree, multiple sources of defaults, repeated type declarations drifting from their schema, or many registrations manually kept in sync.

Choose the existing owner or a shared source of truth, migrate its consumers, and check behavior at the points where the implementations differed. Avoid a configurable universal helper whose complexity exceeds the duplication it removes.

## Abstractions and module boundaries

Trace one real operation end to end. Look for pass-through layers that hide ownership, circular or reversed dependencies, lower layers importing product-specific identifiers, and modules combining unrelated responsibilities. Judge by the coupled decisions and consequences, not line counts or a preferred architectural style.

Read the rationale for extension seams and adapters. A single current implementation may still isolate a volatile dependency or enforce a valuable contract. Remove a layer only if callers become clearer and the separation it supplied is unnecessary or preserved elsewhere.

## Error handling and trust boundaries

Inspect where data becomes trusted and who observes failure. Differentiate redundant internal checks from input validation, authorization, resource ownership, and defenses against stale or partial external state. Review retries, deduplication, transaction boundaries, cancellation, and cleanup during failure.

An empty catch, success-shaped fallback, or broad cast can hide a real failure. Fix the contract and observable error path rather than removing handling to shorten the function. Retain existing privacy constraints in logs and diagnostics.

## Tests and verification

Look for important unprotected behavior, assertions that only mirror mocks, tests that pass when the behavior is broken, flaky timing, and gaps between unit fixtures and production composition. For relevant changes, test outcomes on the real application path with deterministic substitutes for external systems.

For permissions, retries, scheduling, and delivery, select the applicable denied actor, wrong scope, duplicate event, partial failure, and recovery cases. Do not manufacture an unrelated testing project during a narrow cleanup. Preserve useful tests even when the implementation they protected becomes simpler.

## Dependencies, configuration, and documentation

Confirm whether overlapping packages really perform the same responsibility and whether an apparently unused dependency supplies CLI, build, peer, or runtime behavior. Unused-package removal and a major-version upgrade are different tasks; do not bundle speculative upgrades into simplification.

Check docs, examples, comments, and environment settings against implemented code and executable commands. Preserve reasons, invariants, and operational workarounds; remove narration and correct stale claims. Fix generated documentation at its source. Design proposals do not establish current behavior.

## When evidence is incomplete

Name the unknown consumer or contract and the evidence needed to resolve it. Keep the code in place, lower confidence, and continue with independent findings. Do not convert uncertainty into either a deletion or a confident claim of safety.

## Sources of the workflow

This skill synthesizes the following approaches in original instructions; none is a runtime dependency:

- [Desloppify](https://github.com/peteromallet/desloppify): persistent findings and an iterative scan, triage, execute, and verify cycle.
- [AI Slop Cleaner](https://github.com/Yeachan-Heo/oh-my-claudecode/blob/main/skills/ai-slop-cleaner/SKILL.md): behavior protection and focused cleanup passes.
- [Architecture Audit](https://github.com/khasky/awesome-agent-skills/blob/main/skills/awesome-architecture-audit/SKILL.md): evidence for boundary problems and documentation drift.
- [Cursor Deslop](https://github.com/cursor/plugins/blob/main/cursor-team-kit/skills/deslop/SKILL.md): small branch-scoped cleanup of incidental AI patterns.
- [Sentry Code Simplifier](https://github.com/getsentry/skills/blob/main/skills/code-simplifier/SKILL.md): clarity and maintainability while preserving functionality.
