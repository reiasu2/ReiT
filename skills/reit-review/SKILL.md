---
name: reit-review
description: Use for local code review, reviewing implemented work before merge, or handling review feedback with technical rigor and scoped fixes.
---

# ReiT Review

Use this skill when work needs review or when responding to review feedback. It combines the useful parts of Superpowers requesting/receiving code review without requiring subagents.

## Modes

### Local Review

Use after major features, complex fixes, or before merge.

Review for:

- Correctness and regressions
- Scope creep
- Missing tests or weak verification
- Error handling and edge cases
- Public API/schema compatibility
- Security-sensitive handling
- Maintainability and consistency with existing patterns

Output findings first, ordered by severity, with file/line references when available.

### Review Feedback Handling

Use when the user gives review comments.

Process:

1. Read the feedback literally.
2. Inspect the referenced code or behavior.
3. Decide whether the feedback is valid.
4. If valid, fix the smallest scope that satisfies it.
5. If questionable, explain the technical reason and propose an alternative.
6. Verify the affected behavior.

## Severity

- **Critical:** correctness, data loss, security, crash, broken build.
- **Important:** likely bug, missing required behavior, weak test for risky code.
- **Minor:** clarity, style, maintainability, optional cleanup.

## Rules

- Do not perform agreement theater; verify the issue.
- Do not expand scope beyond the review comment unless needed for correctness.
- Do not ignore Critical or Important findings.
- If no issues are found, say so and note remaining test gaps.


## ReiT Testing Call Marker

During ReiT testing, every final reply shaped by this skill MUST end with exactly one call marker line.

Use one of these formats:

`Used skills: reit, reit-review`

or, if no ReiT skill or ReiT sub-skill was actually used:

`Used skills: none`



