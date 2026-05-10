---
name: ReiT-brainstorming
description: Use for lightweight requirement clarification, product or architecture exploration, implementation approach comparison, UI direction, and pre-implementation thinking when full ReiT brainstorming is not yet needed.
---

# ReiT Brainstorming

Use this skill when the user is exploring what to build, how to implement something, or which direction to take. It starts lightweight, then escalates into ReiT full brainstorming mode when the task is large or high-risk. It does not depend on the original Superpowers plugin.

## When to Use

Use for:

- Unclear requirements
- Multiple possible implementation approaches
- Product, UI, workflow, architecture, or technical direction choices
- Explicit brainstorming requests such as "brainstorm" or "brainstorm ideas"
- "How should we do this?" conversations
- Pre-implementation thinking before a medium task

Do not use for:

- Straightforward commands or clearly scoped edits
- Pure code review
- Bug reproduction or root-cause debugging, where `ReiT-debugging` is better
- Tasks where the user explicitly asks to implement immediately and the scope is already clear

## Core Rule

Keep brainstorming proportional to task risk.

- Small task: ask 0 to 1 key question, then recommend a path.
- Medium task: give 2 to 3 approaches, tradeoffs, and a recommendation.
- Large or high-risk task: switch inside this skill to **ReiT Full Brainstorming Mode**.

Do not force a written spec, commit, or per-section user approval unless the user asks or the project requires it.

## Process

### 1. Minimal context pass

Read only the context needed to avoid guessing:

- Existing repo patterns if editing a repo
- Relevant docs or existing files
- Current user constraints

Avoid broad scans unless the direction depends on system-wide structure.

### 2. Classify scope

Classify as:

- **Small:** One clear change, low risk, direct verification.
- **Medium:** Several files, visible behavior changes, or multiple viable designs.
- **Large:** Cross-cutting architecture, public contracts, schema, auth, persistence, concurrency, or ambiguous product direction.

### 3. Ask only if needed

Ask a question only when the answer changes the plan materially and cannot be inferred.

For small tasks, ask at most one question. If risk is low, state an assumption and continue.

### 4. Present options

For medium or uncertain tasks, present:

- Goal understanding
- Key uncertainty
- Option A
- Option B
- Optional Option C
- Recommendation
- Next step

Keep options concrete. Explain tradeoffs in terms of implementation cost, risk, maintainability, UX, and verification.

### 5. Transition

If the user says "OK", "continue", "looks good", or gives equivalent approval:

- Small task: implement directly with targeted verification.
- Medium task: hand off to `ReiT-plan` for execution order unless the scope is already narrow and obvious.
- Large task: switch to ReiT Full Brainstorming Mode before code.

## Output Shape

Use this concise structure when helpful:

```markdown
**Understanding**
[One short paragraph.]

**Uncertainty**
[Only the important uncertainty, or "No key uncertainty".]

**Options**
- A: [approach, tradeoff]
- B: [approach, tradeoff]
- C: [optional]

**Recommendation**
[Recommendation and why.]

**Next Step**
[Implement / ask one question / write short plan / switch to full workflow.]
```

For very small tasks, do not force this structure. A short recommendation is enough.

## ReiT Full Brainstorming Mode

Use this mode when the task is too large or risky for lightweight brainstorming.

Full mode still stays pragmatic. It means "formal enough to avoid bad implementation", not "always write long documents".

### Full mode triggers

Switch to full mode when any of these are true:

- Public APIs, schemas, persistence, auth, infra, CI, or multiple subsystems are involved.
- Product decisions materially affect architecture.
- Several teams, repos, modules, or long-lived workflows are affected.
- The implementation would be hard to verify without an explicit design.
- The user asks for a formal design, spec, plan, or architecture proposal.

### Full mode process

1. State that the task needs full mode and why.
2. Gather only the missing context that affects the design.
3. Ask focused questions one at a time when needed.
4. Present 2 to 3 viable approaches with tradeoffs.
5. Recommend one approach and explain the decision.
6. Produce a concise design covering:
   - Goal
   - Non-goals
   - Scope and boundaries
   - Components or files likely involved
   - Data flow or control flow
   - Risks
   - Verification plan
7. If the user approves, write either:
   - a short execution plan for medium work, or
   - a formal plan/spec only when durable documentation is useful.

### Full mode output shape

```markdown
**Why Full Mode**
[Risk or scope reason.]

**Goals / Non-goals**
[Concrete target and exclusions.]

**Option Comparison**
- A: [approach, tradeoff]
- B: [approach, tradeoff]
- C: [optional]

**Recommended Design**
[Chosen approach, components, flow, boundaries.]

**Verification**
[How to prove it works.]

**Execution Entry**
[Start implementation / write plan / ask one missing question.]
```

## Boundaries

Escalate from lightweight to ReiT full mode when:

- User asks for a formal design/spec
- The project needs durable design docs
- The task affects public APIs, schemas, persistence, auth, infra, CI, or multiple subsystems
- There are unresolved product decisions that materially affect architecture
- The plan would be hard to verify without a written spec

Otherwise stay lightweight.


## ReiT Testing Call Marker

During ReiT testing, every final reply shaped by this skill MUST end with exactly one call marker line.

Only mark this skill when it was actually activated and its brainstorming workflow was used. A quick list of suggestions without scope classification, options, and recommendation does not count.

Use one of these formats:

`Used skills: ReiT, ReiT-brainstorming`

or, if no ReiT skill or ReiT sub-skill was actually used:

`Used skills: none`




