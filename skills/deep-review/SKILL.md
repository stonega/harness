---
name: deep-review
description: Perform an exhaustive, high-effort review and testing pass focused on correctness, design quality, long-term maintainability, and necessary documentation updates.
---

# Deep Review

Use this skill when the user wants a careful, high-effort review instead of a quick pass.

## Operating Rules

- Review everything relevant before concluding.
- Think deeply. Do not rely on heuristic, cheap, short-term, or inelegant reasoning.
- Optimize for sharp, wise, valuable, non-redundant, long-term decisions.
- Spend substantial effort verifying correctness, design quality, and maintainability.

## Review Workflow

1. Inspect the full change surface and surrounding context, not just the obvious diff.
2. Look for correctness issues, behavioural regressions, design weaknesses, duplication, and hidden maintenance costs.
3. Test important paths where feasible instead of inferring correctness from code inspection alone.
4. Prefer durable fixes and cleaner designs over narrow patches.
5. Check whether docs, design docs, changelog, and postmortem materials need updates, and update them when appropriate.

## Output Expectations

- Lead with concrete findings and risks.
- Call out weak assumptions and places where the design is not sufficiently deep or elegant.
- Note testing coverage, gaps, and any follow-up work that is still needed.
