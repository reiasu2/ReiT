# Global Agent Rules

Use `ReiT` as the default personal workflow skill.

ReiT is Rei's personal thinking layer. It keeps the useful engineering discipline from Superpowers as reference material, but the active workflow is self-maintained by ReiT. Lightweight tasks stay lightweight; medium, ambiguous, complex, or high-risk tasks route to focused ReiT sub-skills.

## Startup Rules

- At the start of each task, use `ReiT` to classify task size, risk, verification level, skill routing, parallelism, and worktree needs.
- If Codex does not auto-load personal skills, use this file as the fallback routing rule.
- Explicit user instructions, repository rules, and current-session constraints take priority.
- Original Superpowers is reference material only, not the default runtime.

## Fallback Routing

- Small tasks: do the direct change with targeted verification; do not default to formal design, formal planning, worktrees, or heavy review.
- Requirements, options, UI, product, or architecture direction: use `ReiT-brainstorming`; complex cases upgrade internally to ReiT Full Brainstorming Mode.
- Medium, ambiguous, multi-step, multi-file, or explicit plan-first tasks: use `ReiT-plan`.
- Complex or multi-task work with independent boundaries: use `ReiT-orchestrator`. Its main purpose is to save main conversation context. The current conversation is the controller brain for dispatch, tracking, integration, and final verification. If the split is artificial, use no-spawn mode.
- Bugs, failures, unexpected behavior, build failures, or test failures: use `ReiT-debugging`.
- Before claiming fixed, complete, passing, ready, or mergeable: use `ReiT-verification`.
- Code review or review feedback: use `ReiT-review`.
- Branch, worktree, and parallel-work closeout: use `ReiT-worktree`; broad daily/repo closeout can use `worktree-closeout`.

## Codex Plan Mode Compatibility

- Codex Plan Mode has higher priority than `ReiT-plan`; when Plan Mode is active, plan only and do not edit files or execute implementation work.
- In Plan Mode, medium, ambiguous, multi-step, multi-file, or explicit plan-first tasks should use the `ReiT-plan` style and quality bar.
- If ReiT-plan shapes a Plan Mode reply, end with `Used skills: ReiT, ReiT-plan`.
- When Codex returns to Default mode, `ReiT-plan` may use the plan-first-then-implement route.

## Safety Rules

- Do not run destructive commands, delete files, rewrite Git history, push, merge, rebase, delete worktrees, or change remotes unless explicitly requested.
- Do not hardcode secrets, credentials, tokens, or API keys.
- Do not concatenate untrusted input into shell commands or SQL.
- Rei has enabled automatic orchestration for complex or multi-task work, primarily to save main context. Spawn subagents only when `ReiT-orchestrator` finds independent, bounded, low-conflict tasks that materially reduce main-context load. Do not specify a model by default.
- Before completion claims, run directly relevant verification. If key verification cannot run, explain why and lower the completion claim.

## Testing Call Marker

- ReiT is currently in testing mode.
- Every final reply must end with one English usage marker: `Used skills: ReiT`, `Used skills: ReiT, ReiT-plan`, or `Used skills: none`.

## Compatibility Additions

- If the user provides `plan2go=<path>`, treat that file as the current execution-plan source. Read it before planning. Do not overwrite it unless explicitly asked.
- Before claiming completion, preparing a commit, preparing a push, or preparing a PR, use `ReiT-verification` and its Change Delivery Gate.
- Commit message format: `<type>(scope): <summary>`. The summary should be concise, imperative, and should not end with a period.
- Repeatedly useful lessons can be documented in project docs or a user-specified notes location. Minimum template: title, trigger signal, root cause or constraint, correct practice, verification method, applicable scope.

## Workflow Output

No fixed default workflow output directory is configured. For cross-project notes, daily summaries, or session summaries, use the user-specified destination. If no destination is specified, ask before writing.

Installed skills:

- `ReiT`
- `ReiT-brainstorming`
- `ReiT-plan`
- `ReiT-debugging`
- `ReiT-verification`
- `ReiT-review`
- `ReiT-orchestrator`
- `ReiT-worktree`
- `research-note-wrap`
- `session-wrap`
- `commit-daily-summary`
- `project-daily-summary`
- `worktree-closeout`

Main skill file: `~/.codex/skills/ReiT/SKILL.md`.
Reference source: `original-superpowers-source/skills/`.
