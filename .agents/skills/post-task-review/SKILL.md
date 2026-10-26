---
name: post-task-review
description: Use once at the end of an approved implementation pass only when that pass modified project source code, test code, or maintained executable scripts. Do not use for planning, documentation-only work, analysis, diagnosis, standalone review or QA, Git-only work, or tasks with no code changes.
---

# Post-Task Review

## Activation

* Run this review only after an approved implementation pass has actually modified project source code, test code, or project-maintained executable scripts, and the coding work for that pass is complete.
* Do not activate it for planning, plan review, documentation, prompts, Skills, AGENTS files, configuration-only changes, `.gitignore` changes, research, status reporting, or Git-only operations.
* Do not activate it for read-only analysis, diagnosis, standalone code review, audit, QA, release-gate checks, or any other task whose purpose is already to inspect rather than implement code.
* Reviewing existing code does not trigger another Post-Task Review. If the user later approves fixes and those fixes modify code, run this review once after that fix pass is complete.
* For a mixed implementation that changes both code and supporting files, run the review because code changed, but keep the review scoped to the approved implementation and its relevant effects.

## Review

* Read plans, requirements, project overviews, progress records, source files, diffs, tests, and other evidence only as needed for the review. Prefer targeted searches and relevant sections over loading complete files by default, and reuse current reliable context when it is sufficient.
* Perform this review once, after coding is complete and before reporting the implementation pass complete. Do not run it during implementation or after intermediate steps.
* Review functional correctness, completeness, regressions, boundary conditions, error handling, resource usage, protocol and persistence behavior, and consistency with the approved plan.
* Keep this review read-only. Do not modify code while reviewing. List supported findings with their evidence, impact, and recommended action, then wait for user approval before fixing them.
* Do not invent issues or report speculative concerns merely to produce findings. If no material issue is found, state that clearly and report any remaining validation limits or residual risks.
* Report a finding only when it has a concrete code path, violated requirement, reproducible scenario, or technically supported failure condition.
* For each finding, provide its location, trigger condition, impact, severity, and recommended action.
* Separate confirmed defects from validation limitations and residual risks.
* Style preferences and hypothetical concerns are not findings unless they violate an approved requirement or create a concrete maintenance risk.

## Review Output

* If material findings exist, list each with location, trigger, impact, severity, evidence, and recommended action. Do not fix findings without approval.
* If no material issue is found, state that clearly and separately report any validation limitations or residual risks.
