---
name: no-mistakes-supersession
description: >-
  Agent-only procedure for safely superseding an in-flight no-mistakes validation run when a
  current explicit captain instruction completely invalidates the work being validated.
  Load before cancelling, recovering branch custody from, or replacing work under an active run.
user-invocable: false
metadata:
  internal: true
---

# no-mistakes-supersession

`AGENTS.md` section 7 "Validate" owns the boundary rule: only a current, explicit captain instruction that completely invalidates the work being validated keeps the task with the same worker instead of routing it to follow-up work or handing it to a replacement.
This skill owns the exact recovery procedure once that boundary is crossed.

1. The worker cancels the active run through no-mistakes axi's supported abort command and confirms through `axi status` that the run has stopped before changing any code.
2. The worker then follows `branch_sync.next_action` from structured `axi status`: use `axi sync`'s supported guarded recovery only when its code is `recover_custody`, and otherwise proceed only when structured status confirms that branch ownership is already returned and no recovery is required.
3. Custody recovery settles branch ownership, not content: the worker must replace the obsolete work from the correct pre-invalidation base rather than building on top of the recovered-but-obsolete head, keeping the obsolete run's own pipeline-fix commits out of what gets validated and shipped.
4. Apart from that single supported abort, the worker must not hand-edit, commit, restart, or start a second validation run while the obsolete run still owns the branch.
5. Once ownership is settled, validate exactly once against that final head so no obsolete or intermediate head is ever treated as authoritative.
