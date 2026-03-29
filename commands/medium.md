---
description: Run the medium lane through the orchestrator for medium-complexity feature work with lightweight discovery and manager-planned execution
agent: orchestrator
---

Treat `$ARGUMENTS` as the user's request for this repo.

Run the medium lane.

Use this lane for work that likely touches multiple files or layers and needs more structure than `/quick`, but does not justify the heavier design and critique loop of `/feature`.

Workflow:

1. Intake
   - Treat the request as a medium-complexity feature candidate.
   - Clarify only if scope, acceptance criteria, or boundaries are materially ambiguous.

2. Lightweight discovery
   - Delegate narrowly scoped repo mapping to `@discovery` for likely edit targets, related modules, existing patterns, and validation hints.
   - Stop once you have enough evidence to scope the change safely.

3. Goal setting
   - Define the concrete behavior change, affected surfaces, scope boundaries, assumptions, and non-goals.

4. Scoped implementation approach
   - Propose a compact repo-fit approach with likely files, symbols, constraints, and validation paths.
   - Highlight the main decisions, assumptions, and risks first.
   - Use the `question` tool to confirm or revise the scoped plan before handing off to track planning.

5. Feature-manager handoff
   - Delegate the approved scoped approach and repo evidence to `@feature-manager`.
   - Ask for builder-ready tracks sized for medium-complexity work.
   - Prefer a `superpowers:subagent-driven-development` style handoff with explicit track order, review checkpoints, and verification.
   - Present the returned tracks concisely with ordering and verification.
   - Use the `question` tool to confirm or revise the execution plan before final edit approval.

6. Approval gate
   - Present the final approved plan concisely.
   - Use the `question` tool to ask for exactly one final `[y/N/edit]` before any delegated edits begin.

7. Execute and monitor
   - After approval, delegate each approved track to `@builder`.
   - Run dependent tracks in order unless the plan clearly marks them as independent.
   - Keep each builder assignment limited to the approved track.
   - If a builder reports ambiguity, blocked progress, or `PIVOT_REQUIRED`, stop execution and reassess before continuing.

8. Wrap up
   - Report implementation status, changed files, validations run, and residual risks or follow-up items.

Guardrails:

- Keep the process lighter than `/feature` and more structured than `/quick`.
- Prefer extending existing patterns over inventing new structure.
- If discovery shows the task is actually too small or too large, explicitly recommend `/quick`, `/debug`, or `/feature`.
