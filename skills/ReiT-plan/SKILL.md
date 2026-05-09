---
name: ReiT-plan
description: Use for ReiT plan-first execution planning when a task is medium, large, ambiguous, multi-step, multi-file, or explicitly asks to walk a plan before implementation.
---

# ReiT Plan

Use this skill when ReiT decides an implementation needs a plan before editing. It is adapted from Superpowers planning and execution skills, but keeps two modes: short plan for medium work and formal plan for large or risky work.

## When to Use

Use for:

- "walk a plan", "plan first", "follow the plan"
- Medium implementation work with several steps
- Multi-file or cross-module changes
- Ambiguous tasks where implementation order matters
- High-risk behavior changes that need explicit verification

Do not use for tiny, single-point edits where a short implicit plan and targeted verification are enough.

## Mode Selection

### Lightweight Plan

Use when the task is medium but bounded.

Output:

```markdown
**Goal**
[One sentence.]

**Scope**
[Files/modules likely touched.]

**Steps**
1. [Concrete step]
2. [Concrete step]
3. [Concrete step]

**Verification**
[Commands or checks.]
```

Then implement if the user already asked for execution and the plan does not expose a blocker.

### Full Plan

Use when the task is large, high-risk, or explicitly asks for formal planning.

Include:

- Goal and non-goals
- Current context
- Approach and boundaries
- Ordered tasks
- Files or modules likely touched
- Test and verification plan
- Risks and rollback/closeout notes when relevant


## plan2go Input

If the user provides `plan2go=<path>`:

1. Treat that file as the current execution plan source.
2. Read it before creating or changing any plan.
3. Keep the live plan aligned with that file's intent and task order.
4. If the file is missing, unreadable, contradictory, or unsafe, stop and report the blocker.
5. Do not overwrite the plan file unless the user explicitly asks to update it.

## Plan Quality Rules

- Make the plan decision-complete.
- Avoid placeholders like TBD, TODO, "add appropriate handling", or "write tests".
- Prefer exact commands when known.
- Name concrete acceptance checks.
- Keep steps small enough to execute safely.
- Do not force TDD for every task; use it when behavior risk justifies it.
- Do not force commits unless the user asks or the repo workflow requires them.


## Codex Plan Mode Compatibility

Codex Plan Mode is a system-level mode and has higher priority than this skill.

When Codex Plan Mode is active:

- Use ReiT-plan as the planning style and quality bar.
- Do not edit files or execute implementation work.
- Produce a decision-complete plan that can be implemented after Plan Mode ends.
- Mark the final reply as `Used skills: ReiT, ReiT-plan` when this skill shaped the plan.

When Codex is in Default mode and the user already asked for implementation, use ReiT-plan to plan first, then proceed if the plan exposes no blocker.

## Execution Handoff

After planning:

- Small or medium plan with clear scope: proceed to implementation when the user asked to implement.
- Large or risky plan: present the plan and wait if any user choice remains.
- If subagents are explicitly requested, split tasks by non-overlapping ownership.

## Self-Review

Before using the plan:

1. Check each user requirement is covered.
2. Remove vague placeholders.
3. Confirm tests/verification match the risk.
4. Confirm no unsafe action is hidden inside implementation steps.


## ReiT Testing Call Marker

During ReiT testing, every final reply shaped by this skill MUST end with exactly one call marker line.

Use one of these formats:

`Used skills: ReiT, <this-skill-name>`

or, if no ReiT skill or ReiT sub-skill was actually used:

`Used skills: none`




