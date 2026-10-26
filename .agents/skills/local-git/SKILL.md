---
name: local-git
description: Use when initializing or operating local Git for the confirmed primary project, including scope checks, staging, commits, checkpoints, or safe local Git recovery. Never includes GitHub remote operations.
---

# Local Git Workflow

This skill governs local Git only. GitHub and other remote operations are outside this skill and require the separate `github-sync` skill plus explicit user authorization.

## Repository Scope

* Local Git is used only for the primary project code that is actually being developed and delivered for the current project.
* The Codex workspace may contain multiple directories, including reference projects, reference source code, SDK examples, datasheets, customer materials, historical projects, experiments, tools, utilities, and other supporting files. The workspace root is not automatically the Git repository root.
* Before any Git operation, identify the primary project code root from the confirmed task context and the code that is actually being modified.
* Inspect project files only when the current Git operation needs a fact that is not already reliably available; prefer targeted searches and relevant sections.
* Run `git init` only inside that primary project code root. Do not initialize Git at the workspace root merely because the workspace contains the project.
* If the primary project code root cannot be identified with confidence, stop and ask the user before initializing Git or staging files.
* Reference materials must remain outside the repository scope whenever practical. Do not copy or move reference content into the repository merely to make it available to Git.

## Files Eligible for Local Git

* Track and commit only files that belong to the primary project and are required for its maintained source code, confirmed project configuration, or confirmed deliverables.
* A file is eligible only when it is project-owned or project-maintained and the development team is permitted to modify it. “Permitted to modify” means maintained by the project workflow, not merely writable at the filesystem level.
* Never exclude a file merely because a directory is named `lib`, `library`, `libraries`, or `sdk`. Project-maintained `.c`, `.h`, assembly, configuration, script, and project files under those directories must be tracked when the team is permitted to modify them.
* Exclude vendor or chip-SDK files that the project is not allowed to modify, even when bundled inside the primary project directory. This includes immutable or precompiled libraries such as `.a`, `.lib`, `.so`, and `.dll`, SDK tools, and generated object or firmware outputs.
* When excluded SDK content is needed to build or flash the project, keep the exact SDK version and acquisition or setup instructions in maintained project documentation rather than tracking the vendor payload.
* Do not commit reference projects, reference source code, SDK samples used only for comparison, datasheets, customer materials, historical projects, unrelated examples, temporary experiments, or analysis-only documents.
* Do not commit external development tools or utility software used only for programming, flashing, conversion, debugging, testing, packaging, inspection, or other support work.
* By default, exclude tool binaries, installers, archives, and similar utility packages such as `.exe`, `.dll`, `.msi`, `.zip`, `.7z`, and equivalent files unless the user explicitly confirms that a specific file is part of the project deliverable.
* Project-owned scripts such as `.bat`, `.cmd`, `.ps1`, `.py`, or other scripts may be committed only when they are an actual maintained part of the primary project workflow, not merely a third-party or reference tool.
* Do not commit build outputs, generated artifacts, temporary files, caches, logs, IDE caches, editor state, passwords, API keys, tokens, private keys, certificates, credentials, or other sensitive information.
* Do not assume that every file under the Codex workspace belongs to the project.

## Local Commit Workflow

* After the current task plan is approved, automatically run `git init` inside the confirmed primary project code root if that project is not already a Git repository. No separate approval is required.
* Automatically create a clearly scoped local commit after completing a task when meaningful project changes were made. The user does not need to request the local commit separately.
* For long-running, risky, or multi-step tasks, create local checkpoint commits at logical boundaries when useful. A checkpoint may contain partial progress, but its commit message must clearly describe its state.
* Before staging, verify that the intended files are not excluded and that excluded, generated, vendor, secret, or unrelated files are not being staged. If `.gitignore` is missing or unsuitable, report the issue instead of redesigning it implicitly.
* Inspect Git status and the staged file list before every commit. Stage explicit files or paths unless the entire repository scope has already been verified, and preserve unrelated user changes.
* Stage and commit only files belonging to the primary project and current approved task.
* Report created commit hashes and whether each commit is final or a checkpoint.
* Do not use destructive Git operations such as `git reset --hard` or `git clean` without the user's explicit approval.

## Remote Boundary

* Local Git commits are part of the normal project workflow. GitHub synchronization is not.
* Do not create GitHub repositories, add or modify remotes, push, force-push, rewrite remote history, publish commits, or otherwise perform remote Git operations unless the user explicitly requests GitHub synchronization.
* When the user explicitly requests GitHub synchronization, stop the local Git workflow and follow the `github-sync` skill before performing any remote operation.

## Skill Boundary

* If the requested action would create or modify a remote, push commits, create a GitHub repository, force-push, or otherwise publish data remotely, stop local-Git execution and apply the `github-sync` skill only after explicit user authorization.
