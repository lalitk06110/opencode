---
description: Run the feature lane through the orchestrator with discovery, critique, manager-planned build tracks, and approval gates
agent: orchestrator
---

Treat `$ARGUMENTS` as the user's request for this repo.

Run the feature lane.

Use this lane for broader, riskier, or more architecture-sensitive feature work that needs explicit discovery, design checkpoints, critique, and narrow builder delegation.

Workflow:

1. Intake
   - Clarify only what materially changes scope, acceptance criteria, or risk.

2. Discovery
   - Delegate codebase mapping to `@discovery`.
   - Ask for existing patterns, likely edit points, validation paths, and major risk areas.

3. Initial plan
   - Draft an initial implementation direction with likely edit targets, validation approach, and major risks.
   - Present it concisely with the main decisions first.
   - Use the `question` tool to confirm or revise the plan before detailed design.

4. Detailed design
   - Draft a concrete implementation approach that fits existing repo patterns.
   - Prefer extending current abstractions over introducing new ones.
   - Present tradeoffs and open risks concisely.
   - Use the `question` tool to confirm or revise the design before critique.

5. Adversarial review
   - Send the design to `@reviewer`.
   - If the work is unusually complex, you may use a second `@reviewer` pass.
   - Incorporate only evidence-backed objections and call out unresolved tradeoffs.
   - Present the reviewed design concisely.
   - Use the `question` tool to confirm whether to proceed, revise, or stop before execution planning.

6. Build plan
   - Delegate track planning to `@feature-manager` using the approved design and repo evidence.
   - Prefer a `superpowers:subagent-driven-development` execution shape with narrow tracks and explicit review checkpoints.
   - Expect a builder-ready plan with as many narrow tracks as needed.
   - Prefer explicit ordered steps, exact files, symbols, constraints, dependencies, and verification.

7. Approval gate
   - Present the final execution plan concisely with track ordering and verification.
   - Use the `question` tool to ask for exactly one final `[y/N/edit]` before any delegated edits begin.

8. Execution
   - After approval, allocate the approved tracks to `@builder`.
   - Execute dependent tracks in order unless the plan clearly marks them as independent.
   - Keep each builder assignment limited to the exact approved track.
   - If any builder returns `PIVOT_REQUIRED`, stop execution, reassess, and revise the plan before continuing.

9. Diagnostics and docs
   - If a specialist cleanup or documentation subagent exists in this repo and the approved workflow needs it, delegate narrowly.
   - If that specialist is unavailable, note the gap explicitly instead of inventing hidden process.

10. Wrap up
   - Invoke `superpowers:requesting-code-review` when the lane includes substantial implementation risk.
   - Report changed files, validations run, residual risks, and obvious next steps.

Guardrails:

- Preserve the granularity of the approved `@feature-manager` plan instead of collapsing it into broad workstreams.
- Prefer simple sequential execution when that reduces coordination risk.
- Do not merge multiple micro-tracks into a broader builder assignment just to reduce the number of runs.
