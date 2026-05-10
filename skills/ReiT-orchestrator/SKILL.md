---
name: reit-orchestrator
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

## Decision Order

Use this order every time. It prevents orchestration from becoming accidental busywork.

1. Apply hard no-spawn rules.
2. Honor explicit user requests for multi-agent, parallel, delegation, or split-agent work by entering this skill.
3. Apply the trigger score.
4. Choose a mode: no-spawn, explore, worker, verifier, or mixed.
5. Identify the controller's immediate local critical-path task before spawning anyone.

Hard no-spawn rules:

- The split would be artificial.
- The next step depends on one immediate blocker that the controller must solve locally.
- The root cause is unknown and sequential debugging is still required.
- Shared contracts, schema, migrations, root config, CI, dependencies, or main entry points are the primary change.
- Two or more agents would need to edit the same file, shared type, config source, or behavior boundary.

When a hard no-spawn rule applies, still use no-spawn mode if this skill was explicitly requested or helpful for context budgeting.

## Trigger Score

Use orchestration when at least two of these are true:

- The task has 3 or more meaningful subtasks.
- The task spans multiple files/modules with clear ownership boundaries.
- Reading the needed context would consume a lot of the main conversation.
- A read-only exploration can run while implementation continues.
- Verification/log inspection can run independently.
- The user explicitly asks for multi-agent, parallel, delegation, or asks to split work across agents.

An explicit user request enters this skill even if the score is low, but it does not force spawning. If spawning would be unsafe or wasteful, choose no-spawn mode and state why briefly.

## Operating Modes

- **Explore mode:** dispatch read-only explorers to summarize large files, APIs, histories, or options.
- **Worker mode:** dispatch workers for disjoint implementation slices with explicit write ownership.
- **Verifier mode:** dispatch verification agents to inspect tests/logs/diffs while the controller continues integration.
- **No-spawn mode:** keep work local, but still use this skill's planning and context budget rules.
- **Mixed mode:** combine explore, worker, and verifier only when their scopes are independent.

Prefer explore mode before worker mode when the system shape is unclear.

### No-Spawn Mode

Use no-spawn mode when orchestration thinking is useful but spawning would not save context or would raise integration risk.

No-spawn flow:

1. State the reason for no spawning in one short sentence when useful.
2. Keep the task local.
3. Use compact notes instead of long inline exploration.
4. Reconsider spawning only if independent side work appears later.

## Context Budget Rules

Use subagents to reduce main-context load when they can work independently.

Good delegation targets:

- Large file or module reading where the controller only needs a summary
- Independent implementation slices with disjoint write sets
- Test/log/CI inspection that would otherwise flood the main context
- Secondary verification while the controller continues integration work
- Comparing several independent options or modules where only the conclusion matters

Keep in the main conversation:

- Final architecture decisions
- Shared contracts, schemas, dependency choices, and root config decisions
- Cross-task conflict resolution
- Final integration and completion claims

Subagent outputs should be compact: findings, changed files, verification run, blockers, and next action. Avoid dumping long logs unless the controller explicitly needs them.

Delegate only when the expected saved context is meaningful:

- Reading large files or several modules would otherwise dominate the main conversation.
- Logs, test output, or CI output would be long enough to obscure the decision path.
- Independent exploration can produce a concise recommendation while the controller makes progress elsewhere.
- The subagent can return a summary or patch without requiring continuous controller guidance.

Controller should maintain a compact ledger:

```text
Task | Owner | Scope | Status | Blocking? | Output needed
```

Keep the ledger in the main conversation when there are 2 or more active subagents.

Default delegation budget:

- Start with 1 to 3 subagents.
- Use 4 only when scopes are clearly independent and the controller can integrate them safely.
- If more than 4 subtasks exist, batch them and keep the later batch pending until the first returns.

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
Do not touch: shared contracts, schemas, dependencies, root config, CI, migrations, or main entry points without controller approval after `reit-plan`
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

Shared-boundary rule:

- Ordinary workers must not touch shared contracts, schemas, dependencies, root config, CI, migrations, or main entry points.
- If a shared-boundary change is required, the controller pauses worker dispatch and routes through `reit-plan`.
- A specialist may inspect shared-boundary code read-only, but write ownership stays with the controller unless explicitly assigned after planning.

## Coordination Loop

1. Decide mode: explore, worker, verifier, or no-spawn.
2. Identify the controller's immediate local task.
3. Spawn only useful subagents with task packets.
4. Continue local non-overlapping work immediately.
5. Wait only when blocked on a subagent result.
6. Review returned changes or findings.
7. Resolve conflicts centrally.
8. Run `reit-verification` before completion.
9. Close subagents that are no longer needed.

## Integration Rules

- Trust but verify subagent results.
- Do not paste long subagent output into the final answer.
- Re-read changed files or diffs before claiming integration.
- If two subagents touched the same behavior, the controller decides the final shape.
- If a subagent reports scope expansion, route back through `reit-plan`.
- If root cause is still unclear, route to `reit-debugging`.

## Failure Handling

If a subagent fails, expands scope, conflicts with another edit, or finds unrelated breakage:

1. Stop relying on that subagent for success claims.
2. Record the smallest useful finding in the ledger.
3. Close or stop waiting on the subagent if it is no longer useful.
4. Route the controller back to the right workflow:
   - `reit-debugging` for unclear root cause or failing behavior.
   - `reit-plan` for scope growth, shared-boundary changes, or integration risk.
   - `reit-verification` for final proof after integration.
5. Do not spawn additional agents to compensate until the controller has resolved the failure mode.

## Output and Call Marker

When this skill shapes the work, the final reply must include:

`Used skills: reit, reit-orchestrator`

If other ReiT skills were used, append them:

`Used skills: reit, reit-orchestrator, reit-plan, reit-verification`


