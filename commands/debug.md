---
description: Run the debug lane through the orchestrator from evidence to root cause to a narrow fix
agent: orchestrator
---

Treat `$ARGUMENTS` as the user's bug report or debugging request for this repo.

Run the debug lane.

Use this lane when the work is primarily about understanding symptoms, finding a root cause, and driving the smallest safe fix.

Workflow:

1. Triage
   - Invoke `superpowers:systematic-debugging` before proposing fixes.
   - Clarify expected vs actual behavior, scope, regression signal, and definition of done only when those facts materially affect the fix.

2. Map the failure
   - Delegate to `@discovery` for entrypoints, suspect files, repro paths, relevant tests, and ranked root-cause candidates unless the user already supplied enough evidence.

3. Hypothesis
   - State the most likely root cause and the evidence supporting it.
   - Name one falsification check so the fix does not become guesswork.
   - Present the hypothesis concisely with the main decision first.
   - Use the `question` tool to confirm or revise the hypothesis before finalizing the fix plan.

4. Fix plan
   - Write the smallest targeted change set with exact files, symbols, constraints against drive-by refactors, and the most relevant verification.

5. Critique
   - Use `@reviewer` when the risk is non-trivial.
   - Present the fix plan concisely, including affected files, verification, and any review-driven changes.
   - Use the `question` tool to confirm whether to proceed, revise, or stop before final edit approval.

6. Approval gate
   - Present the final approved plan concisely.
   - Use the `question` tool to ask for exactly one final `[y/N/edit]` before any delegated edits begin.

7. Execution
   - After approval, delegate the plan to `@builder`.
   - If the builder reports `PIVOT_REQUIRED`, stop and re-plan from evidence.

8. Verify and wrap
   - Confirm the most relevant repro path or failing test is addressed.
   - Report what was fixed, what remains uncertain, validations run, and any follow-up checks.

Guardrails:

- Keep the narrative evidence-first.
- Prefer one primary hypothesis over a long list.
- Do not broaden the fix into unrelated cleanup unless the evidence shows it is required.
