---
name: reit-debugging
description: Use for bugs, test failures, build failures, unexpected behavior, performance issues, and integration problems where ReiT should find root cause before changing code.
---

# ReiT Debugging

Use this skill before fixing a technical problem. It adapts Superpowers systematic debugging into a practical ReiT loop.

## Core Rule

No fix before evidence.

Do not patch symptoms until you can state the likely root cause and the evidence supporting it.

## Modes

### Lightweight Debugging

Use for simple, localized failures.

1. Read the exact error or symptom.
2. Reproduce or inspect the failing state.
3. Check recent local changes or nearby code.
4. Identify the smallest plausible cause.
5. Apply one fix.
6. Verify the original symptom.

### Full Debugging

Use for flaky, multi-component, repeated, or unclear failures.

1. Read full errors, logs, stack traces, and exit codes.
2. Reproduce consistently, or gather enough evidence if reproduction is not possible.
3. Trace data/control flow backward from the failure.
4. Compare with a working path or reference implementation.
5. Add temporary diagnostics only when needed and remove them before completion.
6. Change one thing at a time.
7. Verify the original symptom and relevant regressions.

## Evidence Checklist

Before proposing a fix, know:

- What failed
- Where it failed
- What input/state reached that point
- What changed recently or differs from the working path
- Why the proposed fix addresses the source, not just the symptom

## Stop Conditions

Stop and ask or report when:

- The failure cannot be reproduced and no logs/evidence are available
- Fixing requires changing public API, schema, auth, infra, or data migration
- Multiple plausible root causes remain and the next diagnostic action has side effects
- Verification cannot be run and the next step would require claiming the issue is fixed

## Final Report

When done, report:

- Root cause
- Fix made
- Verification run
- Residual risk, if any


## ReiT Testing Call Marker

During ReiT testing, every final reply shaped by this skill MUST end with exactly one call marker line.

Use one of these formats:

`Used skills: reit, reit-debugging`

or, if no ReiT skill or ReiT sub-skill was actually used:

`Used skills: none`



