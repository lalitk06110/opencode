---
description: Run the quick lane through the orchestrator for small isolated changes with one tiny plan and one build track
agent: orchestrator
---

Treat `$ARGUMENTS` as the user's request for this repo.

Run the quick lane.

Use this lane only when the task is small, local, and low-risk enough that one focused builder track can finish it safely.

Workflow:

1. Quick scope check
   - Confirm the request is narrow enough for one builder run.
   - If it is broader, riskier, or better handled as bug work, say so and recommend `/medium`, `/feature`, or `/debug` instead of forcing it through this lane.

2. Minimal context gathering
   - Delegate to `@discovery` only when you need exact file targets, symbols, or validation hints.
   - Keep discovery narrow and avoid broader planning work.

3. Tiny plan
   - Produce one compact execution track with the change goal, exact files when known, constraints, and the smallest useful verification.
   - Highlight the main decisions first.
   - Use the `question` tool to confirm or revise the plan before moving to edit approval.

4. Approval gate
   - Present the final one-track plan concisely.
   - Use the `question` tool to ask for exactly one final `[y/N/edit]` before any delegated edits begin.

5. Execute
   - After approval, delegate exactly one focused implementation track to `@builder`.
   - Do not open multiple build tracks.
   - If the builder reports ambiguity or `PIVOT_REQUIRED`, stop and reassess instead of expanding scope.
   - If a specialized cleanup or documentation subagent is mentioned elsewhere but unavailable in this repo, call that out explicitly rather than improvising hidden process.

6. Wrap up
   - Report files changed, validations run, residual risks, and any reason the task should have been escalated out of the quick lane.

Guardrails:

- Keep the lane fast and compact.
- Do not skip the approval gate before delegated edits.
- Do not add feature-design or adversarial-review steps unless you first escalate the task to another lane.
