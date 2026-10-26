# AGENTS.md

Project: PZ-Kirin-F407 (STM32F407ZGT6) learning & development platform.
This file is adapted from the generic AGENTS template for a resource-constrained 32-bit MCU firmware project. Project-specific confirmed rules take precedence over generic examples. The generic template declares a company-level GitHub boundary (all repositories in the `guofengsoft` organization, `Private`). For this project the owner explicitly overrode that boundary on 2026-10-26: GitHub sync uses the personal-account repository `hdasidhsidasd/-F407---T300-` (Public). See Section 17.

## 1. Task Requirement Log

* Use the confirmed primary-project-root `chats.md` to append meaningful user requirements, changes, constraints, corrections, and decisions, preserving the user's wording where practical.
* After plan approval and before the first state-changing action, create the primary-project-root `chats.md` if needed and append the current task's meaningful requirements with an accurate timestamp.
* Before implementing later additions or changes, append only the new meaningful content.
* Before implementation, confirm that the applicable approved requirements and maintained project documentation are current and consistent. If the approved change affects documented behavior, contracts, or project facts, update the relevant documentation and resolve contradictions before implementing dependent code. Do not update unrelated documentation or silently choose between conflicting sources.
* Do not record read-only investigation, planning, simple acknowledgements, or execution commands unless they add a requirement or materially change scope.
* Append only. Do not rewrite history or invent requirements or timestamps.

## 2. Plan and Approval Gate

* Before each new task, perform read-only investigation and present a reviewable plan covering the goal, scope, non-goals, affected areas, validation, risks, and any decisions required from the user.
* For complex work, include ordered checkpoints with concrete work, affected scope, expected output, and acceptance evidence.
* Until the user explicitly approves the latest plan, do not edit files, install dependencies, build, test, launch applications, publish, or otherwise change state.
* A user instruction may serve as the approved plan only when it explicitly says that it approves the current bounded workflow step and fully defines its allowed actions, files, non-goals, and validation. Any scope expansion still requires a revised plan and approval.
* Approval applies only to the latest unchanged plan. If a user request or new evidence materially changes scope, protocol behavior, persistence, dependencies, risks, or acceptance criteria, revise the plan and request approval again.
* Any issue found during investigation, implementation, testing, validation, or review that is not already inside the explicitly approved fix scope must be reported with evidence and impact and must receive explicit user approval before repair. Approval of one finding does not authorize fixing other findings.

## 3. Goal-Based Task Execution

* Classify each task during planning as simple or complex based on scope, affected areas, dependencies, implementation risk, and validation burden.
* A simple task requires a concise plan and explicit user approval before implementation. It does not require a Goal or `PROGRESS.md` unless its scope grows materially.
* For a complex task, propose one Goal in this form: `/goal Complete [objective] until [test and acceptance conditions] are all satisfied.` The Goal and plan must cover the approved objective, scope, acceptance conditions, and complete ordered checkpoints. Do not change state before the user explicitly approves the plan.
* After approval, create or refresh the confirmed primary-project-root `PROGRESS.md` with the approved Goal, acceptance conditions, and every checkpoint's work, scope, expected output, acceptance evidence, and status.
* Update `PROGRESS.md` immediately when a checkpoint completes or when confirmed scope, evidence, validation, or blockers change. Material changes require a revised plan and renewed explicit user approval before further implementation.
* Before resuming an active Goal, first use the current reliable context. Read the relevant `PROGRESS.md` section, inspect Git status or diff, or check validation results only when needed to establish the current checkpoint or refresh evidence that may have changed. Continue from the first incomplete checkpoint; do not repeat completed work unless its evidence is missing or invalidated.
* For behavior that can be reliably automated, use TDD: first write a failing test, then implement the minimum code needed to pass it, and finally clean up under passing tests. When TDD is not practical for hardware, UI, integration, or legacy constraints, define the validation method before implementation instead of creating artificial tests.
* Retry the same confirmed error no more than twice after its initial failure. After that, change the approach or record the unresolved condition instead of repeating the same attempt.
* Complete the Goal only after every checkpoint, acceptance condition, applicable validation, and any Post-Task Review required by its activation conditions. If a human decision is required, record `Blocked` in `PROGRESS.md` with the confirmed facts, impact, required decision, and next action.

## 4. No Guessing

* Do not guess or invent code behavior.
* Every function, variable, macro, struct field, register bit, timer, callback, and hardware behavior must be confirmed from existing code or user-provided information.
* Before calling, modifying, or relying on a function, confirm its declaration and its implementation or authoritative SDK/API contract, actual purpose, return type, exact return-value and error-code semantics, side effects, valid inputs, ownership, and execution context. Do not infer any of these from the function name or a call site.
* For STM32 registers and peripheral behavior, confirm against the reference manual (`STM32F4xx中文参考手册.pdf`), the datasheet (`STM32F407ZGT6数据手册.pdf`), or the actual library implementation (`stm32f4xx_*.c/.h`). Do not assume bit meanings, clock source, or reset values without evidence.
* When implementing or changing a function, verify that every return path matches the confirmed contract. At each call site, confirm whether the return value must be checked or may be ignored, handle it accordingly, and preserve its full type and range. Do not assume that `0`, nonzero, negative values, `true`, `false`, `NULL`, or an enum means success or failure without evidence.
* Resolve uncertainty through read-only code inspection first. Ask the user only when a material ambiguity remains and different assumptions would change behavior, scope, compatibility, persistence, or risk.
* Do not create fake APIs or call functions that are not confirmed to exist.

## 5. MCU Code Style

* Target: STM32F407ZGT6, Cortex-M4F @168MHz, toolchain KEIL MDK (AC5/armcc), standard peripheral library V1.5.1 and/or HAL (STM32CubeF4 V1.25.0).
* Avoid recursion, dynamic allocation, large stack buffers, heavy abstraction, and blocking logic.
* Keep ISRs, timer callbacks, DMA completion callbacks, RT-Thread/FreeRTOS tasks, and main-loop code short and predictable.
* Follow KISS and implement the smallest complete solution that satisfies the approved requirement and fits the existing architecture. Do not omit required validation, error handling, diagnostics, compatibility, or safety, and do not add speculative abstractions, features, refactors, or future-proofing.
* For new or materially changed project-maintained code, keep a function within 200 lines and a source file within 1000 lines where practical. Functions over 300 lines or files over 1500 lines must be split unless they are generated, vendor, or data-table files. Do not refactor unrelated legacy code only to meet these limits.
* Do not reformat whole files.

## 6. Data Types and Math

* Follow the project's existing integer-type conventions and use the smallest width that safely covers the confirmed range.
* Use fixed-width types from `<stdint.h>` (`uint8_t/uint16_t/uint32_t`) for register and protocol values; use `volatile` for hardware registers and shared ISR/main variables as confirmed.
* Prefer integer or fixed-point math. Use `float` or `double` only when planned and justified for the target (FPU is available on Cortex-M4F, but fixed-point is still preferred unless FPU use is confirmed).
* Check range limits, overflow, underflow, truncation, and signed/unsigned conversion.

## 7. Code Marking and Comments

When modifying code, add a necessary marker near the changed block using the confirmed name and actual modification date:

```c
// <confirmed-name> YYYYMMDD
```

Code marking rules:
* Mark changed logic blocks, not every line.
* Do not spam markers.
* If the change affects several separated places, mark each changed area once.
* Keep existing useful comments.
* Follow the project's existing comment style if different.
* The confirmed name for this project is `李乐` unless corrected in PROJECT_OVERVIEW.md.

Comment rules:
* When adding new features or modifying existing logic, add appropriate comments where they help explain the purpose, behavior, or important constraints.
* Comments should explain why the code exists or what key behavior it controls.
* Do not repeat obvious code line by line.
* Do not over-comment simple code.

## 8. Interrupts, Callbacks, and Shared State

* Keep ISR and time-critical callback work minimal. Do not block, allocate memory, write persistent storage, or perform heavy processing there.
* Treat interrupts (EXTI/TIM/USART/SPI/I2C/DMA), callbacks, timers, RT-Thread/FreeRTOS tasks, deferred work, and main-loop interactions as asynchronous unless the confirmed execution model proves otherwise.
* Confirm execution context, priority, reentrancy, shared-state ownership, and shared-state lifetime before changing related logic.
* Review shared variables and multi-step state transitions for race conditions, lost updates, torn reads or writes, stale callbacks, cancellation and timeout ordering, `volatile`, atomicity, and required critical sections.
* Keep interrupt-disabled sections as short as possible.

## 9. Buffers and Protocol Safety

* Validate packet lengths, indexes, array bounds, payload ranges, command values, and null termination before accessing data.
* Preserve confirmed byte order, field widths, checksums, framing, and backward compatibility.
* Reject or safely ignore malformed, truncated, oversized, and unsupported input.
* Do not copy protocol data into fixed-size buffers without an explicit bound.
* Do not change protocol semantics or externally visible behavior without explicit approval.
* For this project, apply to USART frames, CAN frames, SPI (W25Qxx) command sequences, I2C (AT24C02/MPU6050) transactions, SDIO/FATFS, and infrared NEC decoding.

## 10. Timers and State Machines

* Check timer wraparound, zero values, maximum values, delayed callbacks, repeated events, and state re-entry.
* Prefer wraparound-safe unsigned time comparisons when consistent with the existing platform.
* Confirm initialization, reset, disconnect, reconnect, timeout, and cancellation behavior.
* Avoid introducing blocking delays into event-driven or timing-sensitive paths.
* For this project, apply to SysTick, general-purpose timers (TIM2-TIM5), input capture, output compare/PWM, and any protocol state machines (e.g., NEC IR decode, I2C bit-bang, button scanning).

## 11. Persistence Integrity (Internal Flash / EEPROM / SPI-Flash)

* Before changing persistence, confirm the existing save/load mechanism, storage layout, field sizes, addresses, versions, and defaults. Do not invent storage APIs or identifiers.
* Internal Flash: sectors must be erased before programming; confirm sector address, size, and flash unlock/lock sequence. Do not write while code executes from the erased region.
* AT24C02 (I2C): confirm page size (8 bytes), page-write boundary behavior, and addressing.
* W25Qxx (SPI): confirm sector/block erase granularity, write-enable (0x06) sequence, and status-register polling.
* Do not write from ISRs, time-critical callbacks, or high-frequency paths. Use the project's confirmed deferred or coalesced save path and avoid unnecessary writes that increase flash wear.
* Preserve compatibility and handle erased, invalid, partial, legacy, and power-loss states safely.
* Changes to storage layout, addresses, formats, versioning, migration, or recovery behavior require explicit plan approval.

## 12. After Editing

* Before completion, check the changed scope for unused code, duplicate logic, temporary implementations, stale comments or tests, and contradictions with maintained documentation. Do not expand this check into unrelated cleanup.
* Report changed files and logic, diagnostic log points and master macro, whether `chats.md` was updated, actual validation, compile and flash status, the engineer-executed validation still required, and unresolved questions or risks.
* State whether logging-enabled and logging-disabled configurations were validated. Never claim a build, test, or flash that was not actually performed.

## 13. Post-Task Review

* Perform one automatic post-task review only after an approved implementation pass has actually modified project source code, test code, or maintained executable scripts, and before reporting that coding pass complete.
* Do not activate the review for planning, documentation-only work, prompts, AGENTS or Skill changes, configuration-only changes, `.gitignore` work, investigation, diagnosis, standalone review or QA, Git-only work, or tasks with no code changes.
* The review is read-only. Do not silently fix findings during the review.
* Before performing the review, read and follow the `post-task-review` skill.
* Do not invent findings. Report only technically supported defects, validation limits, or residual risks.

## 14. Engineer-Executed Validation Handoff

* After one complete code implementation pass for the current task, the applicable Post-Task Review, and approved fixes are complete, give the engineer a short numbered list of exactly what to do and what result to expect at each step.
* Use confirmed project facts already available in the current context (chip STM32F407ZGT6, board PZ-Kirin-F407, toolchain Keil MDK, download via PZ-ISP or ARM emulator, serial log via USART1 115200). Do not ask the engineer to repeat stable project information for every task. Ask only when a missing or changed fact prevents an accurate test instruction.
* Start with the changed behavior, then add only the relevant boundary, asynchronous, failure, recovery, or regression checks. State when a diagnostic log must be enabled and which related output should be returned.
* Keep a project-root `TESTING.md`. Record the test steps provided after each implementation pass and, after the engineer reports back, add a concise summary of the actual result and any problem found. Preserve earlier entries so they can be referenced when preparing the project's final overall test.

## 15. Filesystem Safety

* Full filesystem access grants technical capability; it does not authorize expanding the scope of a destructive operation.
* Before any recursive, wildcard-based, or multi-item delete or move, resolve every target to an absolute path and verify its exact scope.
* Refuse the operation if a target is empty, unresolved, any filesystem or volume root, a UNC share root, the user profile root, the workspace root, or an ancestor of the workspace.
* Do not construct destructive paths from unvalidated variables, globs, string interpolation, or command substitution. In PowerShell, prefer `-LiteralPath` and never use patterns such as `Remove-Item "$target\*"`.
* For recursive or bulk PowerShell deletions, run the command with `-WhatIf` first and inspect the resolved targets before performing the actual deletion.
* Destructive operations outside the current workspace require an explicit user request and verification of the exact absolute target.
* Prefer recoverable operations, such as moving files to a workspace-local quarantine directory, when practical. If the target or scope is ambiguous, stop and ask the user.

## 16. Local Git Core Rules

* Before initializing Git, staging, committing, or performing other local Git operations, read and follow the `local-git` skill.
* Create or change `.gitignore` only in the dedicated project workflow step or an explicitly approved `.gitignore` task; do not redesign it implicitly during an unrelated local Git operation.
* Local Git applies only to the confirmed primary project code root and only to project-maintained files that the development team is permitted to modify.
* This project root contains a large reference/learning archive (PDFs, videos, SDK packages, tools, experiment zips). None of these become repository content merely because they are visible. The repository holds only maintained project documentation and flow files unless a source-code scope is explicitly approved.
* Local commits are part of the approved development workflow. They never authorize GitHub or other remote operations.

## 17. GitHub Remote Boundary

* Do not perform any GitHub or other remote operation unless the user explicitly requests it for the current project.
* Before creating a repository, changing a remote, pushing, or renaming a repository, read and follow the `github-sync` skill.
* Project-specific override (confirmed by the owner, 2026-10-26): the generic company rule (all repositories in the `guofengsoft` organization and `Private`) is NOT applicable to this project. The confirmed target for this project is the personal-account repository `hdasidhsidasd/-F407---T300-` (Public). Do not switch targets or change its visibility without explicit owner approval.
* Never infer authorization for force-push, remote-history rewriting, repository deletion, visibility changes, or unrelated branches.
* Never upload passwords, API keys, tokens, certificates, private keys, credentials, or other sensitive information.

## 18. Diagnostic Logging

* Automatically add sufficient diagnostics to new or changed runtime paths, using the project's confirmed logging backend and conventions (USART1 printf redirection via `fputc`, baud 115200).
* Control all added logs with one confirmed compile-time master macro (e.g., `#define LOG_ENABLE 1`). When disabled, logs must not evaluate arguments, call helpers, change state, or retain avoidable logging code and strings.
* Log failures, invalid state or input, protocol and persistence errors, hardware failures, timeouts, buffer and queue violations, overflow or underflow, truncation, dropped events, unexpected ranges, and important state transitions or results.
* Include enough stable context to identify the event, reason, state, actual value, expected limit, and result, without exposing secrets or unnecessarily dumping payloads.
* Prevent flooding in repeated paths, and never perform blocking formatted output in ISRs or time-critical callbacks; defer, count, summarize, or rate-limit instead.
* Check format specifiers, integer widths, buffer and stack use, and reentrancy against the actual toolchain. Apply the normal person-and-actual-date marker to logging changes.
* When authorized and practical, validate both logging-enabled and logging-disabled configurations and report the exact master macro and test value.
