---
name: ReiT-verification
description: Use before claiming work is complete, fixed, passing, ready, or mergeable. Requires fresh verification evidence appropriate to the task risk.
---

# ReiT Verification

Use this skill before any completion claim. It is ReiT's self-maintained completion gate, inspired by Superpowers verification discipline.

## Core Rule

Evidence before claims.

Do not say work is complete, fixed, passing, ready, or mergeable without fresh verification evidence from this turn.

## Verification Levels

- **Level 0: Targeted check** for low-risk local edits.
- **Level 1: Focused regression** for bug fixes and local behavior changes.
- **Level 2: Red/green or equivalent** for high-risk behavior changes.
- **Level 3: Review pass** for major features or complex fixes.
- **Level 4: Completion gate** before final completion claims.

## Gate

Before a success claim:

1. Identify what would prove the claim.
2. Run the relevant command or manual check.
3. Read the full output and exit code.
4. Compare output against the claim.
5. Report the result honestly.

## What Counts

Good evidence:

- Test command with passing output
- Build/lint/typecheck command with exit 0
- Manual reproduction of the original symptom
- File/content inspection proving a requested artifact exists
- Checklist against explicit requirements

Weak evidence:

- "It should work"
- Prior test run from before the latest change
- Partial logs that do not cover the claim
- Agent/subagent report without local inspection


## Change Delivery Gate

Before declaring work complete, preparing a commit, preparing a push, or preparing a PR, confirm:

1. Directly relevant verification has run and results are reported honestly.
2. The appropriate ReiT quality gate was used for the task risk.
3. Repository-required checks take priority when stricter than ReiT defaults.
4. If key verification cannot run, state why and lower the completion claim.

Do not use passing unrelated checks as proof for the requested change.

## If Verification Cannot Run

Say:

- What could not be run
- Why
- What was checked instead
- What risk remains


## ReiT Testing Call Marker

During ReiT testing, every final reply shaped by this skill MUST end with exactly one call marker line.

Use one of these formats:

`Used skills: ReiT, <this-skill-name>`

or, if no ReiT skill or ReiT sub-skill was actually used:

`Used skills: none`



