---
name: reit-worktree
description: Use for deciding when to use a git worktree, managing branch/worktree hygiene, or closing out parallel work without automatically merging, deleting, pushing, or pruning.
---

# ReiT Worktree

Use this skill for branch, worktree, and parallel-work hygiene. It adapts useful Superpowers worktree and branch completion ideas, but keeps all risky Git actions explicit.

## When to Use

Use for:

- Deciding whether a task needs isolation
- Planning work across branches or worktrees
- Closing out work after implementation
- Summarizing branch/worktree state
- Preparing merge/PR/cleanup options

Do not use for simple edits on the current branch unless isolation materially reduces risk.

## Worktree Decision

Use a worktree when:

- The task is large or risky
- Current workspace has unrelated dirty changes
- Multiple independent efforts need parallel progress
- The task may require disruptive dependency/build changes

Avoid a worktree when:

- The change is small and scoped
- The current branch is already the intended workspace
- Setup cost exceeds risk reduction

## Closeout Checklist

Before saying a branch/worktree is done:

1. Inspect status and diff.
2. Identify what changed and why.
3. Verify relevant tests/checks.
4. Summarize remaining risks.
5. Present safe next actions: keep, PR, merge, archive, or delete.

## Safety Rules

- Do not merge, rebase, push, prune, or delete without explicit user instruction.
- Do not delete worktrees with uncommitted or untracked work unless the user explicitly accepts data loss.
- Do not clean unrelated worktrees.
- Do not rewrite Git history unless explicitly requested.
- Prefer reporting exact commands over running risky commands.

## Relationship to worktree-closeout

Use `worktree-closeout` when the user wants a broader closeout scan or daily/repo-level closeout appendix.

Use this skill for the decision logic and safety rules around a single task, branch, or worktree.


## ReiT Testing Call Marker

During ReiT testing, every final reply shaped by this skill MUST end with exactly one call marker line.

Use one of these formats:

`Used skills: reit, reit-worktree`

or, if no ReiT skill or ReiT sub-skill was actually used:

`Used skills: none`



