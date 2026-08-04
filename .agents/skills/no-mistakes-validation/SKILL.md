---
name: no-mistakes-validation
description: >-
  Agent-only procedure for supervising an in-flight no-mistakes validation run: routing new
  requirements that arrive mid-run, relaying an ask-user decision back to the worker, and judging
  or correcting the run's current state.
  Load whenever a ship task's no-mistakes validation is active and a wake needs to act on it.
user-invocable: false
metadata:
  internal: true
---

# no-mistakes-validation

`AGENTS.md` section 7 "Validate" owns the always-inline boundary facts: firstmate never invokes `no-mistakes axi respond` for a crew-owned run, and only a current explicit captain instruction that completely invalidates the work (`no-mistakes-supersession`) keeps the task with the same worker instead of routing to follow-up work.
This skill owns everything else about supervising the run while it is active.

## Scope-routing while validation is active

Prefer routing new requirements to follow-up work rather than expanding the current task, unless a new requirement completely invalidates the work being validated.
The smallest downstream changes needed to keep already accepted product or engineering behavior correct, add behavioral tests where an executable contract exists, or keep documentation accurate remain within the current task even when they touch files not named at intake.
Corrections required to satisfy already accepted intent are not new requirements.

## Relaying an ask-user decision

Send the same worker one exact decision naming the decision key, step, action, affected finding IDs, instructions where needed, and exact response command, passing `--resolve-key` so the worker's open decision record closes at answer time.
Require the matching `resolved` event, forbid `--yes`, and require the worker to process every synchronous return until completion or a genuinely new escalation.
Resume fleet supervision immediately after the decision lands.

## Judging and correcting run state

Judge validation by the current-code-matched run step through `bin/fm-crew-state.sh`, not by shell liveness or the last status event; its header owns the exact run-step-to-state mapping.
A worker hand-editing, committing, aborting, or restarting during an active validation run duplicates pipeline ownership outside the supersession sequence; steer it back to the gate response flow.
The worker reports the PR when CI first becomes green rather than waiting for merge monitoring to finish.
