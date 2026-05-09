---
name: ReiT-orchestrator
description: Use when a task is complex, multi-part, or naturally parallelizable and ReiT should act as the controller brain that delegates bounded work to subagents, integrates results, and verifies centrally.
---

# ReiT Orchestrator

Use this skill when ReiT should run the current conversation as the controller brain to save main-context space. The controller classifies work, splits independent tasks, dispatches subagents for bounded reading/implementation/verification, tracks state, integrates results, and verifies centrally.

Primary goal: save main conversation context. Subagents absorb isolated code reading, implementation details, and verification logs, then return compact findings or patches. This is not parallelism for its own sake. Still follow system and platform constraints.

## When to Use

Use for:

- Complex tasks with several independent parts
- Multi-file or multi-module work where subtasks have clear ownership
- Parallel research plus local implementation
- Review/verification that can run while implementation continues
- Work where the main conversation should coordinate rather than do every detail inline

Do not use when:

- The task is small or single-threaded
- Changes concentrate in 1 to 2 files
- Shared contracts, schemas, migrations, root config, CI, or dependencies are the main work
- The root cause is unknown and needs sequential debugging first
- Integration risk is higher than saved time

## Trigger Score

Use orchestration when at least two of these are true:

- The task has 3 or more meaningful subtasks.
- The task spans multiple files/modules with clear ownership boundaries.
- Reading the needed context would consume a lot of the main conversation.
- A read-only exploration can run while implementation continues.
- Verification/log inspection can run independently.
- The user explicitly asks for multi-agent, parallel, delegation, or asks to split work across agents.

Do not spawn if the split would be artificial. A single clear local task should stay in the main conversation.

## Operating Modes

- **Explore mode:** dispatch read-only explorers to summarize large files, APIs, histories, or options.
- **Worker mode:** dispatch workers for disjoint implementation slices with explicit write ownership.
- **Verifier mode:** dispatch verification agents to inspect tests/logs/diffs while the controller continues integration.
- **No-spawn mode:** keep work local, but still use this skill's planning and context budget rules.

Prefer explore mode before worker mode when the system shape is unclear.

## Context Budget Rules

Use subagents to reduce main-context load when they can work independently.

Good delegation targets:

- Large file or module reading where the controller only needs a summary
- Independent implementation slices with disjoint write sets
- Test/log/CI inspection that would otherwise flood the main context
- Secondary verification while the controller continues integration work

Keep in the main conversation:

- Final architecture decisions
- Shared contracts, schemas, dependency choices, and root config decisions
- Cross-task conflict resolution
- Final integration and completion claims

Subagent outputs should be compact: findings, changed files, verification run, blockers, and next action. Avoid dumping long logs unless the controller explicitly needs them.

Controller should maintain a compact ledger:

```text
Task | Owner | Scope | Status | Blocking? | Output needed
```

Keep the ledger in the main conversation when there are 2 or more active subagents.

## Controller Role

The current conversation is the controller brain.

Controller responsibilities:

1. Decide whether orchestration is appropriate.
2. Identify the immediate critical-path task to do locally.
3. Split only independent side tasks.
4. Define read/write ownership for every subagent.
5. Keep subagents aware they are not alone in the codebase.
6. Integrate results centrally.
7. Run final verification before success claims.

Do not hand off the immediate blocker if the next local step depends on it.

## Dispatch Rules

Before spawning:

- State the task split briefly.
- Assign each subagent a concrete role.
- Define allowed files/modules or read-only scope.
- Define expected output.
- State stop conditions.

Use this task packet shape:

```text
Role: explorer / worker / verifier
Goal: one concrete outcome
Read scope: files/modules/commands allowed for inspection
Write scope: files/modules allowed to edit, or "read-only"
Do not touch: shared contracts, schemas, dependencies, root config, CI, migrations unless explicitly assigned
Expected output: compact findings / changed files / verification result / blockers
Stop conditions: scope expansion, conflicting edits, missing context, unsafe change, verification failure outside scope
```

Worker prompt must include:

- "You are not alone in the codebase."
- "Do not revert edits made by others."
- "Stay within your assigned files/scope."
- "If you need to change shared contracts, schema, dependencies, root config, CI, or migrations, stop and report."

Prefer:

- Explorers for read-only codebase questions
- Workers for bounded implementation with disjoint write sets
- Verification agents only when they can run in parallel with ongoing work

## Coordination Loop

1. Decide mode: explore, worker, verifier, or no-spawn.
2. Identify the controller's immediate local task.
3. Spawn only useful subagents with task packets.
4. Continue local non-overlapping work immediately.
5. Wait only when blocked on a subagent result.
6. Review returned changes or findings.
7. Resolve conflicts centrally.
8. Run `ReiT-verification` before completion.
9. Close subagents that are no longer needed.

## Integration Rules

- Trust but verify subagent results.
- Do not paste long subagent output into the final answer.
- Re-read changed files or diffs before claiming integration.
- If two subagents touched the same behavior, the controller decides the final shape.
- If a subagent reports scope expansion, route back through `ReiT-plan`.
- If root cause is still unclear, route to `ReiT-debugging`.

## Output and Call Marker

When this skill shapes the work, the final reply must include:

`Used skills: ReiT, ReiT-orchestrator`

If other ReiT skills were used, append them:

`Used skills: ReiT, ReiT-orchestrator, ReiT-plan, ReiT-verification`


