# Global Agent Rules

Use `reit` as the default workflow skill.

ReiT is a Codex workflow layer. It keeps useful engineering discipline from Superpowers as reference material, but the active workflow is self-maintained by ReiT. Lightweight tasks stay lightweight; medium, ambiguous, complex, or high-risk tasks route to focused ReiT sub-skills.

## Startup Rules

- At the start of each task, use `reit` to classify task size, risk, verification level, skill routing, parallelism, and worktree needs.
- If Codex does not auto-load skills, use this file as the fallback routing rule.
- Explicit user instructions, repository rules, and current-session constraints take priority.
- Original Superpowers is reference material only, not the default runtime.

## Fallback Routing

- Small tasks: do the direct change with targeted verification; do not default to formal design, formal planning, worktrees, or heavy review.
- Requirements, options, UI, product, or architecture direction: use `reit-brainstorming`; complex cases upgrade internally to ReiT Full Brainstorming Mode.
- Medium, ambiguous, multi-step, multi-file, or explicit plan-first tasks: use `reit-plan`.
- Complex or multi-task work with independent boundaries: use `reit-orchestrator`. Its main purpose is to save main conversation context. The current conversation is the controller brain for dispatch, tracking, integration, and final verification. If the split is artificial, use no-spawn mode.
- Bugs, failures, unexpected behavior, build failures, or test failures: use `reit-debugging`.
- Before claiming fixed, complete, passing, ready, or mergeable: use `reit-verification`.
- Code review or review feedback: use `reit-review`.
- Branch, worktree, and parallel-work closeout: use `reit-worktree`; broad daily/repo closeout can use `worktree-closeout`.

Routing boundary: `reit-brainstorming` decides what and why, `reit-plan` decides execution order, and `reit-orchestrator` decides whether bounded delegation saves main-context space.

## Invocation Ledger

- Keep a lightweight internal ledger of ReiT skills that were actually activated during the turn.
- A skill counts as activated only when its trigger matched and its `SKILL.md` workflow constraints were used.
- Similar answer style, ordinary local sequencing, or generic reasoning does not count as activation.
- The final `Used skills` marker must be generated from this ledger, not from the shape of the final prose.
- If an explicit skill request is skipped because a higher-priority rule blocks it, say so briefly and do not list that skill as used.

## Codex Plan Mode Compatibility

- Codex Plan Mode has higher priority than `reit-plan`; when Plan Mode is active, plan only and do not edit files or execute implementation work.
- In Plan Mode, medium, ambiguous, multi-step, multi-file, or explicit plan-first tasks should use the `reit-plan` style and quality bar.
- If reit-plan shapes a Plan Mode reply, end with `Used skills: reit, reit-plan`.
- When Codex returns to Default mode, `reit-plan` may use the plan-first-then-implement route.

## Safety Rules

- Do not run destructive commands, delete files, rewrite Git history, merge, rebase, delete worktrees, or change remotes unless explicitly requested.
- Normal pushes for this repository may follow the GitHub push cadence below. Force-pushes or history rewrites still require explicit user approval.
- Do not hardcode secrets, credentials, tokens, or API keys.
- Do not concatenate untrusted input into shell commands or SQL.
- Automatic orchestration is available for complex or multi-task work, primarily to save main context. Spawn subagents only when `reit-orchestrator` finds independent, bounded, low-conflict tasks that materially reduce main-context load. Do not specify a model by default.
- Before completion claims, run directly relevant verification. If key verification cannot run, explain why and lower the completion claim.

## GitHub Push Cadence

- Do not push every small edit by default.
- Batch small related updates and push when the repository is in a coherent checkpoint.
- As a practical default, push after about three related edits, or sooner when the user asks, the change is important to publish, or the local state should be backed up.
- This repository-specific cadence grants permission to create ordinary English checkpoint commits for coherent batched changes.
- Keep commit messages and public repository content in English.

## Public Template Boundary

- This public repository must stay English-only and template-safe.
- Local installations may be customized outside this repository.
- Do not copy local paths, local notes, machine-specific settings, or user-specific data into this public repository.

## Testing Call Marker

- ReiT is currently in testing mode.
- Every final reply must end with exactly one English usage marker.
- Format: `Used skills: reit[, reit-subskill...]` or `Used skills: none`.
- Include every ReiT sub-skill that was activated in the invocation ledger.
- If a task meets `reit-plan` triggers, activate `reit-plan` and include it in the marker even when no separate plan document was shown.
- Do not mark `reit-plan` for ordinary lightweight sequencing or implicit local planning that did not meet `reit-plan` triggers.

## Compatibility Additions

- If the user provides `plan2go=<path>`, treat that file as the current execution-plan source. Read it before planning. Do not overwrite it unless explicitly asked.
- Before claiming completion, preparing a commit, preparing a push, or preparing a PR, use `reit-verification` and its Change Delivery Gate.
- Commit message format: `<type>(scope): <summary>`. The summary should be concise, imperative, and should not end with a period.
- Repeatedly useful lessons can be documented in project docs or a user-specified notes location. Minimum template: title, trigger signal, root cause or constraint, correct practice, verification method, applicable scope.

## Workflow Output

No fixed default workflow output directory is configured. For cross-project notes, daily summaries, or session summaries, use the user-specified destination. If no destination is specified, ask before writing.

Installed skills:

- `reit`
- `reit-brainstorming`
- `reit-plan`
- `reit-debugging`
- `reit-verification`
- `reit-review`
- `reit-orchestrator`
- `reit-worktree`
- `research-note-wrap`
- `session-wrap`
- `commit-daily-summary`
- `project-daily-summary`
- `worktree-closeout`

Main skill file: `~/.codex/skills/ReiT/SKILL.md`.
Reference source: `original-superpowers-source/skills/`.
