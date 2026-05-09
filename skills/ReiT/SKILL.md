---
name: ReiT
description: Use ReiT as a Codex workflow layer. Applies lightweight-vs-full ReiT workflow judgment, task sizing, verification level selection, subagent boundaries, workflow skill routing, multi-agent orchestration, note output location, Git safety, and concise user-facing communication rules.
---

# ReiT

Use ReiT as the default workflow layer before deciding whether to invoke ReiT full workflows. It keeps engineering discipline available, but prevents small tasks from being over-processed.

## Priority

Follow instructions in this order:

1. System, developer, and explicit current user instructions
2. Repository rules, project docs, and local conventions
3. Global or project `AGENTS.md`
4. This skill
5. Individual ReiT or workflow skills

If a lower-priority skill conflicts with a higher-priority instruction, follow the higher-priority instruction.

## Default Mode

Use ReiT workflow discipline. Original Superpowers is only a reference source, not the default runtime.

- Prefer the shortest path that still satisfies quality and verification needs.
- Decide first whether the task is read-only, lightweight, medium, or high-risk.
- For medium, large, ambiguous, or high-risk implementation work, use `ReiT-plan` before editing. For complex or multi-task work with independent parts, use `ReiT-orchestrator` as the controller brain.
- Use a single focused workflow skill when enough.
- Escalate to ReiT full workflow or a focused ReiT sub-skill only when task size, risk, or ambiguity justifies it.
- Keep moving when the user asks to continue, unless blocked by a real missing decision or unsafe action.

## Lightweight Task Policy

Lightweight tasks include single-file or small scoped edits, clear bug fixes, config changes, text changes, small tests, and local docs updates.

For lightweight tasks:

- You may skip formal design, formal implementation plans, worktree isolation, and heavy review chains.
- Use a short implicit plan: goal, boundary, risk, verification.
- Ask at most one key question before starting, and only if the answer cannot be inferred safely.
- If risk is controlled, state the assumption and proceed.
- Do not write design docs, specs, or long plans unless the user asks or the project requires them.
- You may modify directly related app code, tests, local docs, and small support files.

Ask before doing any of these:

- Deleting files
- Large refactors
- Shared contracts, schemas, shared types, public API changes
- Root config, CI, dependency, env template, database, persistence, or infra changes
- Git history changes, remote operations, merges, rebases, pushes
- Changes outside the visible task boundary

## Escalation and De-escalation

Escalate when:

- The affected boundary grows beyond the initial scope
- Public API, schema, persistence, concurrency, shared logic, or auth is involved
- Requirements remain ambiguous after local context review
- Verification coverage is weak for the risk
- The task becomes medium or large implementation work

De-escalate when:

- The change is local and clear
- No shared core logic is affected
- Verification is direct
- A long plan or TDD cycle would cost more than it protects
- The issue has narrowed to a single fix

## Task Routing

### Read-only tasks

For analysis, explanation, architecture notes, code reading, information queries, and reviews without file edits, answer directly with grounded conclusions.

If debugging a real failure but not yet editing, use the ReiT debugging loop: reproduce, gather facts, isolate cause, change one thing, verify.

### Implementation tasks

Implementation includes features, bug fixes, behavior changes, refactors, UI work, APIs, scripts, and data logic.

- Small: use lightweight policy and targeted verification.
- Medium: write a short plan first, then implement.
- Large or high-risk: write a formal plan first, then implement task-by-task.
- Always verify the directly affected behavior before claiming completion.
- If verification cannot be run, explain why and state residual risk.

## ReiT Workflow Routing

Use ReiT sub-skills as the self-maintained workflow set:

- `ReiT-brainstorming`: requirements, design, options, UI/product/architecture direction; escalates internally to full brainstorming mode.
- `ReiT-plan`: medium, large, ambiguous, multi-step, multi-file, or explicit plan-first implementation work.
- `ReiT-debugging`: bugs, failures, unexpected behavior, build/test failures, and root-cause investigation.
- `ReiT-verification`: completion gate before claiming fixed, passing, complete, ready, or mergeable.
- `ReiT-review`: local code review and review feedback handling.
- `ReiT-orchestrator`: complex or multi-task controller mode that saves main-context space by delegating bounded reading/implementation/verification to subagents, then integrates centrally.
- `ReiT-worktree`: worktree/branch decision, hygiene, and safe closeout.

Routing boundary: `ReiT-brainstorming` decides what and why, `ReiT-plan` decides execution order, and `ReiT-orchestrator` decides whether bounded delegation saves main-context space.

## Verification Levels

Choose the level explicitly based on risk.

- Level 0: Targeted verification for small, low-risk changes.
- Level 1: Regression tests for small or medium bug fixes and local behavior changes.
- Level 2: TDD for new behavior, shared logic, and high-risk behavior changes.
- Level 3: Code review for major features, complex fixes, or pre-merge checks.
- Level 4: Completion verification before claiming work is complete, fixed, passing, ready to merge, or ready to ship.

Do not claim "passed", "complete", "ready", or "fixed" without verification evidence.

## ReiT Native Workflows

Use ReiT-native judgment first. Do not depend on the original Superpowers plugin being installed.

- Requirements and design: use `ReiT-brainstorming` for lightweight clarification, option comparison, and pre-implementation thinking.
- Planning: use `ReiT-plan` for medium tasks and formal plans for multi-step, multi-file, or high-risk work.
- Orchestration: use `ReiT-orchestrator` when complex or multi-task work has independent parts suitable for subagents, especially when delegation will save main-context space. Use no-spawn mode if task splitting would be artificial.
- Debugging: use `ReiT-debugging` to reproduce or observe the issue, gather facts, identify the smallest plausible cause, change one thing at a time, and verify.
- TDD: use only for high-risk behavior changes, new shared behavior, or bugs where a regression test is valuable.
- Review: use `ReiT-review` for major features, complex fixes, and review feedback.
- Completion verification: use `ReiT-verification` before declaring work complete.
- Worktrees: use `ReiT-worktree` only when isolation materially helps branch hygiene or risk control.
- Branch closeout: use `ReiT-worktree` for single-branch decisions or `worktree-closeout` for broader closeout scans.

Use installed workflow skills when requested or clearly useful:

- `research-note-wrap`: research notes and synthesis.
- `session-wrap`: session summary and handoff.
- `commit-daily-summary`: commit-based daily summary.
- `project-daily-summary`: project daily summary.
- `worktree-closeout`: worktree, branch, and parallel-work closeout.

During ReiT testing, every final reply MUST end with exactly one call marker line.

Use one of these formats:

`Used skills: ReiT`

or, when sub-skills were used:

`Used skills: ReiT, ReiT-plan`

or, if no ReiT skill or ReiT sub-skill was used:

`Used skills: none`

## Notes and Output Location

Default note and summary output root: use the user-provided path, project-local documentation, or a clearly named notes directory when appropriate.

- Use the user-specified path when provided.
- Put project-specific material in the project when appropriate.
- For cross-project notes, daily summaries, session summaries, and research notes, ask for a destination if the user did not provide one.
- Prefer conclusions, structure, status, and next steps.
- Avoid chat transcript dumps and low-signal process logs.
- Suggest writing durable lessons into a project `AGENTS.md` when useful, but do not edit unrelated projects without user intent.


## Experience Note Template

When a lesson repeatedly proves useful and the user wants it documented, write it to the relevant project docs or the default note root using this minimum structure:

- Title
- Trigger signal
- Root cause or constraint
- Correct practice
- Verification method
- Applicable scope

No fixed default note root is configured; ask for a destination if the user did not provide one.

## Parallelism and Subagents

Automatic subagent orchestration is available for complex or multi-task work, primarily to save main-context space. Use subagents when `ReiT-orchestrator` judges the work has independent, bounded tasks and system/platform rules allow it.

Parallel work is appropriate only when independent tasks have clear read/write boundaries and low integration risk. Default to 1 to 3 subagents, use 4 only for clearly independent scopes, and batch anything larger. The current conversation remains the controller brain: it plans, dispatches, monitors, integrates, and verifies.

Use `ReiT-orchestrator` for the detailed controller-worker protocol: decision order, trigger score, operating mode, task packets, compact ledger, coordination loop, failure handling, and integration rules.

Do not parallelize when:

- Changes are concentrated in 1 to 2 core files
- Shared contracts, shared types, schema, migrations, CI, dependencies, root config, or main entry points are involved
- The root cause is unknown
- Integration risk is higher than the saved time

Subagent rules:

- Define ownership and file boundaries.
- Default to inherited model and reasoning effort unless the user requests otherwise or the task clearly needs an override.
- Do not let two subagents edit the same file, config source, contract, schema, or shared types.
- A subagent must stop and report if it needs to exceed scope, change shared contracts, touch root config, change dependencies, alter CI, add migrations, or if verification fails outside scope.
- After subagents complete, integrate and verify centrally.

## Git and Safety

- Do not run destructive commands unless explicitly requested.
- Do not use non-Git tools to manipulate `.git`.
- Avoid deletion except clearly scoped temporary artifacts.
- Do not hardcode secrets, credentials, tokens, or API keys.
- Use parameterized database queries.
- Do not shell-concatenate untrusted input.
- Do not terminate processes not started for the current task unless explicitly requested.
- Do not merge, rebase, force-push, rewrite history, delete worktrees, or clean unrelated worktrees without explicit user intent.
- Normal pushes may follow repository-specific push cadence rules when the repository explicitly defines them.


## Commit Norms

Use these rules when the user asks to commit or when preparing a commit message:

- Format: `<type>(scope): <summary>`
- `scope` is optional.
- For public repositories, use an English imperative summary and keep public commit messages free of private details.
- For repositories without explicit language rules, follow project conventions.
- Keep the summary concise and do not end it with a period.
- Common `type` values: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`.
- Do not commit unless the user explicitly asks for a commit or the repository defines a checkpoint/push cadence that grants ordinary commit permission.

## Communication

- Match the user's language for chat replies unless a higher-priority repository rule says otherwise.
- Keep public repository artifacts, commit messages, and durable docs in the language required by that repository.
- Use English for code identifiers.
- Lead with the conclusion, then evidence, tradeoffs, and next steps.
- For execution tasks, report current action and next step.
- For analysis tasks, report conclusion, basis, risk, and recommendation.
- During testing, every final reply must end with exactly one call marker: `Used skills: ReiT`, `Used skills: ReiT, <sub-skill>`, or `Used skills: none`.
- Use Markdown code blocks with language tags for multi-line code, config, or logs.
- Use tables only when they make comparison clearer.










