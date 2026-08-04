---
name: dispatch-discipline
description: >-
  Agent-only anti-pattern guards for task intake and delivery-path rigor: when NOT to build new
  tooling, when NOT to run a parallel design exercise, when NOT to serialize overlapping work, and
  when NOT to layer a manual review on top of the selected delivery path.
  Load before proposing new automation, before deciding to serialize dispatch on file overlap
  alone, before dispatching a design-only scout alongside a likely-enough answer, or before adding
  any review step beyond the selected delivery path.
user-invocable: false
metadata:
  internal: true
---

# dispatch-discipline

`AGENTS.md` section 7 owns the always-inline classification and authority boundaries (project resolution, secondmate routing, Ship/Scout definitions, delivery-mode/yolo resolution).
This skill owns the anti-pattern guards that keep dispatch and delivery from growing unneeded process.

## Don't build machinery you don't need

For one-off or infrequent operational work, start with the simplest direct end-to-end path.
Do not build wrappers, control planes, policy layers, custom verifiers, or automation unless the direct path exposes a concrete blocker or repeated need that justifies the added machinery.

## Don't serialize on file overlap alone

Treat file or subsystem overlap as a risk signal rather than an automatic reason to wait, and dispatch isolated work immediately with no concurrency cap when each change can be independently implemented and validated and the selected delivery path can reconcile ordinary rebases or conflicts.
Serialize only for a true semantic dependency, shared mutable external state, incompatible concurrent migration, or another concrete condition that makes independent progress or reconciliation unsafe; same-file editing alone is insufficient, and genuine blockers remain durable.

## Don't scout when an answer already exists

If established evidence already answers an informational question, relay it without a design-only scout; when implementation intent is unclear, answer and ask one concise implementation question when useful rather than dispatching speculative design work.
Never both present a likely-enough solution and launch a parallel design exercise that is not expected to change it.

## Don't layer a manual review on the selected delivery path

The selected delivery path owns its own rigor.
When no-mistakes is selected, no-mistakes alone owns review, fixes, tests, documentation, push, PR, and CI; otherwise follow the faster path without adding an independent reviewer.
Never hold work outside no-mistakes for a manual clean verdict, stack serial manual reviews, or infer authority for one from security, architecture, or risk alone.
`AGENTS.md` section 7 owns the narrow exception (an explicit captain request or a knowledge-only review).
If fast-path risk needs more rigor, escalate whether to use no-mistakes instead of inventing a manual gate.
