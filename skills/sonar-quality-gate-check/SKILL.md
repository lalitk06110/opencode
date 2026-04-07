---
name: sonar-quality-gate-check
description: Analyze a SonarQube project’s quality gate, active issues, coverage risks, duplication, and security hotspots, then produce a concise Markdown remediation report.
---

# Purpose

Use this skill to review a SonarQube project and generate a single actionable report for developers before merge, during review, or during quality audits.

# Use this skill when

- The repository uses SonarQube and you need the current quality gate result.
- You need a remediation report that prioritizes bugs, vulnerabilities, code smells, coverage gaps, and duplication.
- You want a repeatable workflow for quality review.

# Do not use this skill when

- The project is not configured for SonarQube.
- The user only wants one metric or a simple pass/fail check.
- You cannot access SonarQube or the local project configuration.

# Inputs

- `sonar-project.properties` in the current repository, if available.
- Optional user-provided project key.
- Optional output filename. Default: `sonar-quality-report.md`.
- Optional coverage threshold. Default: `80`.

# Outputs

Produce both outputs:

1. A concise terminal summary with:
   - quality gate status
   - failed conditions
   - total active issues
   - hotspot count
   - worst coverage and duplication risks

2. A Markdown report file named `sonar-quality-report.md` unless the user requested another name.

# Workflow

## 1. Resolve the project key

1. Read `sonar-project.properties` from the current working directory.
2. Extract:
   - `sonar.projectKey` (required)
   - `sonar.sources` (optional context)
   - `sonar.tests` (optional context)

3. If the file is missing, look in likely parent directories.
4. If no project key can be found, stop and ask the user for the SonarQube project key.

## 2. Fetch quality gate status

Call `sonarqube_get_project_quality_gate_status` with:

- `projectKey`: resolved project key

Capture:

- overall gate status
- each condition name
- comparator or threshold
- actual value
- pass/fail state

## 3. Fetch project-level measures

Call `sonarqube_get_component_measures` with:

- `projectKey`: resolved project key
- `metricKeys`: `["coverage", "duplicated_lines_density", "ncloc", "violations", "complexity", "code_smells", "bugs", "vulnerabilities", "security_hotspots"]`

Use these values for the report summary.

## 4. Fetch active issues

Call `sonarqube_search_sonar_issues_in_projects` with:

- `projects`: `[projectKey]`
- `issueStatuses`: `["OPEN", "CONFIRMED"]`
- `ps`: `100`
- `p`: `1`

If more pages exist, continue paging until all active issues are collected.

For each issue, capture at minimum:

- severity
- type
- impact quality, if available
- rule or title
- file path
- line number, if available
- message

Group issues by:

- severity: `BLOCKER`, `HIGH`, `MEDIUM`, `LOW`, `INFO`
- type: `BUG`, `VULNERABILITY`, `CODE_SMELL`
- file path

## 5. Fetch low-coverage files

Call `sonarqube_search_files_by_coverage` with:

- `projectKey`: resolved project key
- `maxCoverage`: coverage threshold, default `80`
- `pageSize`: `100`

Sort the returned files from lowest coverage upward.

## 6. Fetch duplicated files

Call `sonarqube_search_duplicated_files` with:

- `projectKey`: resolved project key

Capture overall duplication density and the files with the highest duplication impact.

## 7. Fetch security hotspots

Call `sonarqube_search_security_hotspots` with:

- `projectKey`: resolved project key
- `status`: `["TO_REVIEW"]`

## 8. Prioritize remediation

Assign per-file priority using these rules:

### High

Use `HIGH` if any of the following are true:

- file has a `BLOCKER` or `HIGH` severity issue
- file has a vulnerability
- file coverage is below `50%`
- file is involved in a failed quality gate condition
- file contains a pending security hotspot with material risk

### Medium

Use `MEDIUM` if any of the following are true:

- file has `MEDIUM` severity issues
- file coverage is between `50%` and the threshold
- file has moderate duplication or many code smells

### Low

Use `LOW` if any of the following are true:

- file only has `LOW` or `INFO` issues
- file has minor test gaps
- duplication is small and localized

## 9. Write the report

Write a Markdown report with this structure:

```markdown
# SonarQube Quality Gate Report

- Project: <project key>
- Date: <timestamp>
- Quality Gate: <OK | ERROR | WARN>

## Executive Summary

- Coverage: <value>
- Duplication Density: <value>
- Active Issues: <count>
- Bugs: <count>
- Vulnerabilities: <count>
- Code Smells: <count>
- Security Hotspots To Review: <count>
- Main blockers: <1-3 concise bullets>

## Quality Gate Conditions

| Condition | Threshold | Actual | Status |
| --------- | --------- | ------ | ------ |

## Active Issues by Type

### Bugs

- Count: <count>
- Highest-risk files: <comma-separated list>

### Vulnerabilities

- Count: <count>
- Highest-risk files: <comma-separated list>

### Code Smells

- Count: <count>
- Highest-risk files: <comma-separated list>

## Coverage Risks

| File | Coverage | Notes |
| ---- | -------- | ----- |

## Duplication Risks

| File | Duplication | Notes |
| ---- | ----------- | ----- |

## Security Hotspots

| File | Hotspot | Status |
| ---- | ------- | ------ |

## Prioritized Remediation Plan

### High

1. `<file>` — <issue summary>; <recommended action>

### Medium

1. `<file>` — <issue summary>; <recommended action>

### Low

1. `<file>` — <issue summary>; <recommended action>

## File Action Table

| Priority | File | Problems | Recommended Action |
| -------- | ---- | -------- | ------------------ |

## Next Steps

1. Fix gate-failing issues first.
2. Add tests for the worst uncovered files.
3. Remove or extract duplicated logic in the highest-risk files.
```

# Report-writing rules

- Keep the report concise and decision-oriented.
- Prefer file-level actions over generic advice.
- Mention exact gate failures first.
- Do not dump raw API payloads.
- If there are no issues, still write the report and explicitly state that the project passed with no active findings.

# Error handling

## Missing `sonar-project.properties`

- Search parent directories.
- If still missing, ask for the project key.

## SonarQube request fails

- State which step failed.
- Suggest checking SonarQube connectivity, authentication, or project access.
- Do not fabricate missing metrics.

## Partial data available

- Write the report with the data you have.
- Clearly mark missing sections as unavailable.

# Example invocation flow

1. Resolve `sonar.projectKey`.
2. Get quality gate status.
3. Get project measures.
4. Page through active issues.
5. Get low-coverage files.
6. Get duplicated files.
7. Get security hotspots.
8. Generate `sonar-quality-report.md`.
9. Print a short summary.

# Notes for agent behavior

- Prefer deterministic tool outputs over assumptions.
- Page through all issues when totals exceed the page size.
- Use the coverage threshold consistently across analysis and recommendations.
- Optimize for a useful remediation plan, not for exhaustive narration.
