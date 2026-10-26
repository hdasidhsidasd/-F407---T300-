---
name: github-sync
description: Use only after the user explicitly requests GitHub sync, upload, backup, repository creation, remote setup, push, or repository rename. Sync only with the guofengsoft organization, verify the primary project and existing remote or repository first, and keep every repository Private.
---

# GitHub Remote Synchronization

GitHub is a remote backup and collaboration layer. It is separate from the normal local Git workflow. This skill does not authorize itself: an explicit user request is required before any remote operation.

## 1. Trigger

* Perform GitHub synchronization only when the user explicitly requests it.
* Requests such as “同步到GitHub”, “上传 GitHub”, “GitHub 备份”, or equivalent wording count as authorization for the current project synchronization.
* Synchronization authorization applies only to repositories owned by the `guofengsoft` organization. Never create, connect, or push to a personal account or another organization.
* The `guofengsoft` owner and `Private` visibility requirements are permanent company-level rules. No task, synchronization request, rename request, or other user instruction may override or revise them; refuse and report conflicting requests.
* Do not automatically push after each coding task or local commit.
* Do not infer a recurring schedule. A phrase such as “同步到GitHub” authorizes the current requested synchronization only unless the user explicitly asks for automation.

## 2. Project Scope Inherits Local Git Rules

* GitHub must contain only the confirmed primary project code permitted by the `local-git` rules.
* Upload only project-owned or project-maintained files that the development team is permitted to modify. “Permitted to modify” means maintained by the project workflow, not merely writable at the filesystem level.
* Never exclude an upload solely because a path contains `lib`, `library`, `libraries`, or `sdk`. Project-maintained `.c`, `.h`, assembly, configuration, script, and project files under those directories are eligible when the team is permitted to modify them.
* Do not upload vendor or chip-SDK files that the project must not modify, even when bundled inside the primary project code root. This includes immutable or precompiled libraries such as `.a`, `.lib`, `.so`, and `.dll`, SDK tools, and generated object or firmware outputs.
* Never upload Source Insight project directories matching `*.si4project/` or any content beneath them.
* If excluded SDK content is required to reproduce a build, upload only maintained documentation that records the exact SDK version and acquisition or setup method; do not upload the vendor payload.
* Treat the repository as a core-file and SDK-overlay backup, not a standalone SDK. Maintained documentation must record the exact original SDK version and source, each overlay target path, the merge or replacement procedure, and post-merge validation steps.
* The Codex workspace may contain reference projects, reference source code, datasheets, customer materials, historical code, tools, utilities, SDK examples, experiments, and other supporting files. None of these become eligible for GitHub merely because they are visible to Codex.
* Do not upload reference-only files, reference projects, external tools, tool binaries, installers, archives, build outputs, temporary files, caches, logs, secrets, credentials, or unrelated files.
* By default, exclude utility files such as `.exe`, `.dll`, `.msi`, `.zip`, `.7z`, and similar packages unless the user explicitly confirms that a specific file is a required project deliverable.
* Before the first remote upload, and whenever repository scope changes, verify the local repository scope, `.gitignore`, Git status, and the full tracked file set, not only the currently staged files.
* If commits to be pushed contain excluded vendor, SDK, library, tool, `*.si4project/`, or `/CODEX_HANDOFF.md` content, do not push. Report the exact paths. If removing the content requires rewriting Git history, wait for explicit user approval and never delete the local SDK, tool, or local handoff files merely to remove them from Git.

## 3. Repository and Remote Detection

Before any GitHub operation:

1. Confirm the current primary project code root.
2. Consult project files only when the current remote operation needs a fact that is not already reliably available; use targeted searches and relevant sections.
3. Inspect local Git status and the current branch.
4. Inspect existing remotes with `git remote -v`. Redact embedded credentials from every displayed or reported URL.
5. If `origin` exists, verify that its owner is exactly `guofengsoft` and that it points to the intended repository.
6. Verify that the intended GitHub repository is `Private`. If it is not, do not push or change its visibility; stop and report the policy violation.
7. If any candidate remote points outside `guofengsoft`, do not push, connect, or change it. Report the redacted remote URL and stop.
8. If no suitable remote exists, check whether `guofengsoft` already contains the expected repository before creating a new one.
9. Never create a duplicate GitHub repository merely because the local repository has no configured remote.
10. For an existing remote branch, fetch remote references without merging or rebasing and compare ahead, behind, and divergence state. Stop on unexpected remote commits or divergence; never merge, rebase, force-push, or rewrite history automatically.

## 4. Existing GitHub Repository

If the correct GitHub repository and remote already exist:

* Preserve the existing remote.
* Push only the approved local commits from the primary project repository.
* Do not recreate or transfer the repository. Rename it only through the explicit rename workflow, and never change its `Private` visibility.

## 5. Missing Remote or Repository

If no correct GitHub remote exists:

1. Obtain the exact intended repository name from the approved initialization or upgrade step, or from the confirmed repository identity in `PROJECT_OVERVIEW.md`. Do not derive a new name here.
2. Check whether the `guofengsoft` organization already contains that repository.
3. If it exists, connect only after confirming it is this same project. Otherwise report the conflict and wait for the engineer to provide another name.
4. If it does not exist, create a new repository only when the current synchronization request authorizes the operation.
5. New repositories must be created under `guofengsoft` and set to `Private`.
6. Add the confirmed repository as `origin` and push the current intended branch.

## 6. Repository Rename

* Repository renaming is allowed only when the user explicitly requests the rename and the acting account has the required organization-owner or repository-admin permission.
* Do not rename an existing repository automatically during project upgrade or merely because its legacy name does not match the current convention.
* Before renaming, confirm the explicitly requested new name is available, verify that the repository is the intended project, and inspect likely impacts on GitHub Pages, workflows, external links, badges, scripts, submodules, package references, and any GitHub Action consumed by other repositories.
* If the repository hosts an action referenced as `owner/repository@ref`, stop and report the dependency risk before renaming.
* After a successful rename, update the local `origin` URL to the new `guofengsoft` URL, verify it with `git remote -v`, and update the repository name in `PROJECT_OVERVIEW.md`.
* Do not create a different repository using the old name after a rename, because doing so can invalidate redirects from the old URL.

## 7. Privacy and Secret Safety

* Every company project repository must remain `Private` without exception.
* Never create a `Public` repository or change repository visibility, even when requested.
* Never upload passwords, API keys, tokens, certificates, private keys, production credentials, account files, or other sensitive information.
* Before the first push, inspect tracked and staged files for sensitive or reference-only content.
* If a secret or excluded file has already entered local Git history, do not push it. Stop and report the issue so the history can be handled safely with user approval.

## 8. Remote Safety

* Never force-push unless the user explicitly approves it.
* Never rewrite remote history automatically.
* Never delete a remote repository automatically.
* Never change an existing remote URL unless its current target has been verified as incorrect and the change is approved or clearly required by the explicit synchronization request.
* Preserve unrelated branches, tags, and remote history.
* Do not push unrelated local branches unless the user requests them.

## 9. Completion Report

After synchronization, report clearly:

* GitHub repository used
* confirmed repository owner (`guofengsoft`)
* confirmed repository visibility (`Private`)
* branch pushed
* latest pushed commit
* whether a new GitHub repository was created
* whether a new remote was added or changed
* whether a repository was renamed and whether the local origin was updated
* whether `.gitignore` or repository scope required correction before push
* synchronization result and any unresolved risk
