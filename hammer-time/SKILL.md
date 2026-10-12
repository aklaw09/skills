---
name: hammer-time
description: Review a PRD and proposed design, then plan and implement approved phases through vertical slices, red-green TDD, risk-based independent review, and per-phase commits. Use when the user says "it's hammer time" or asks for this phased implementation workflow.
---

# Hammer Time

Turn a PRD or specification into production code without losing requirements between design review, planning, implementation, review, and delivery.

Do not treat the PRD or its proposed design as complete by default. Inspect the repository and clarify material gaps before finalizing the design or plan. Preserve normal authorization boundaries: approval of one artifact does not authorize unrelated changes or external actions.

## Operating modes and approvals

Use **supervised mode** by default. Analysis artifacts may be created for review, but the user must explicitly approve an artifact before it authorizes a downstream state-changing action. Record each approval in the running decision log as: artifact or phase, approved scope, approved actions, and any limits. A phase approval must explicitly include permission to commit before a commit may occur.

Use **autonomous mode** only when the user explicitly enables it and supplies a predeclared, versioned authorization record. It must name the PRD, design review, phase map, and phase it covers; repository and branch target; expected base commit; permitted actions; scope limits; and required exit criteria. Record the matching authorization entry before every implementation, fix, or commit. Before every state-changing phase action and immediately before committing, verify the repository root, current branch, and HEAD against that record; stop if the target is wrong or HEAD moved unexpectedly. It substitutes for the corresponding supervised approval only when every named condition is satisfied. Treat it as the complete authority; do not infer further permission from a successful run or a broad comment such as "looks good." In either mode, stop and request direction when there is an unresolved material unknown, material scope or design change, public-contract or compatibility change, destructive migration or data operation, security-sensitive change, failed required check, work outside the approved scope, external side effect, production-environment access, deployment, publish/release, or third-party state change. The user must explicitly authorize the exact action, target environment, and scope before any of those operations occur.

## 1. Establish the contract

1. Read the supplied PRD/specification and relevant repository guidance, architecture, existing implementations, tests, schemas, and contracts.
2. Build a traceability list with stable IDs for every story, requirement, constraint, and acceptance criterion, plus dependencies, unknowns, and explicit deferrals.
3. Review the proposed design before planning. Present a recommended design, viable alternatives and why they were rejected, assumptions, compatibility and operational concerns, risks, and unknowns. Mark every unknown as blocking or non-blocking, and identify the decision owner for each blocking one. Do not approve the design while a blocking unknown remains unresolved, unless it is explicitly converted to an approved assumption or deferral with an owner and deadline.
4. Ask only questions whose answers can materially change scope, behavior, architecture, or acceptance criteria. Keep resolved decisions in the decision log.
5. In supervised mode, wait for approval of the design review before creating the execution plan. In autonomous mode, proceed only when the design review is covered by the authorization record.
6. Divide the work into ordered phases that each deliver a coherent, reviewable capability. State dependencies and completion criteria for every phase.
7. Present the full phase map, including explicit deferrals. Assign every traceability ID to one ordered phase, an explicitly approved deferral, or a documented out-of-scope decision; do not approve a map with unassigned IDs. In supervised mode, wait for its approval before implementation. In autonomous mode, proceed only when the phase map is covered by the authorization record.

## 2. Plan one phase at a time

Keep the approved phase map at roadmap level. Focus detailed planning on the next unimplemented phase only. Break it into the smallest useful vertical slices. Each slice must cross the necessary boundaries to deliver observable behavior rather than merely adding a layer or file.

For each slice, specify:

- the user-visible or system behavior;
- the in-scope traceability IDs it delivers or validates, including requirements, acceptance criteria, constraints, and dependencies;
- affected boundaries and likely files;
- the first failing test (red);
- the minimum implementation needed to pass (green);
- refactoring and regression checks;
- acceptance criteria, risks, dependencies, and deferrals.

In supervised mode, show the detailed phase plan to the user and wait for its approval. In autonomous mode, create the detailed plan only within the authorized phase and scope limits, record its version and traceability IDs, and proceed only when the design review, phase map, and implementation action are covered by the authorization record. Plan later phases in detail only when they become current.

When requirements change during planning or implementation:

1. record the new decision;
2. create or update the relevant stable traceability IDs;
3. identify every affected phase, contract, test, migration, dependency, detailed plan, and exit checklist;
4. assign every changed traceability ID to a phase, approved deferral, or documented out-of-scope decision;
5. explain which approved plans must change;
6. obtain approval for material scope or architecture changes before proceeding.

## 3. Implement the approved phase

Implement one phase at a time and do not start the next phase until the current one is reviewed, approved, and committed.

For each vertical slice, follow red-green-refactor:

1. **Red:** add the smallest meaningful test and run it to confirm it fails for the expected reason.
2. **Green:** write the minimum production code needed to pass the test.
3. **Refactor:** remove duplication and improve names or structure without changing behavior.
4. Run focused tests, relevant regression tests, type checks, builds, linting, and schema/contract validation in proportion to risk.

Keep changes inside the approved phase. Preserve unrelated worktree changes. Do not hide failing tests or silently weaken acceptance criteria.

## 4. Independently review the phase

After the phase's initial implementation and tests pass, select independent reviewers from the phase's risk profile. Give each reviewer the phase diff, connected code, acceptance criteria, expected load, and test results. Reviewers report findings only and do not edit code.

1. **Baseline:** use `ponytail:ponytail-review` to check correctness, safety, load, tests, speed, minimality, verbosity, and repetition. The reviewer must read connected callers and dependencies, not only the diff.
2. **Select additions by risk:** choose readability/maintainability review when the change has non-trivial control flow or new abstractions; database review when it reads or writes a database; security review for authentication, authorization, secrets, user input, or sensitive data; API compatibility review for public contracts; performance review for hot paths or material scale concerns; and UI/accessibility review for user-facing changes.
3. **Record applicability:** for each candidate review, state whether it ran and why, or why it was not applicable. Do not claim independence when a distinct reviewer or relevant capability was unavailable.

Do not use a generic combined review to claim that a selected risk-specific review occurred.

## 5. Classify findings and obtain approval

Consolidate duplicate findings and verify each against the code. Report two groups:

- **User input required:** product or contract choices, architecture tradeoffs, material scope changes, destructive migrations, meaningful compatibility changes, or findings with multiple valid outcomes.
- **AI-ready:** clear correctness fixes, missing tests, safe naming/readability improvements, and minimal in-scope deduplication with one evident solution.

For every finding include severity, concrete impact, smallest fix, affected phase(s), and whether it changes an already approved decision. Recommend which fixes are required before the phase can ship.

In supervised mode, wait for the user to approve the proposed fixes. In autonomous mode, implement only fixes that are covered by the authorization record and do not meet a stop condition. Rerun relevant verification, and request another independent review when a fix materially changes behavior, architecture, database access, or public contracts.

## 6. Approve and deliver the phase

Give the user a phase summary containing:

- traceability IDs and their status: completed, deferred, or not completed. Do not call a story completed when one of its required acceptance criteria is deferred;
- tests and builds run with results;
- review outcomes, applicability, and applied fixes;
- remaining risks, deferred work, and unverified production behavior;
- impact on later phases;
- an exit checklist mapping every in-scope traceability ID to a passing automated test, recorded manual check, other objective evidence, explicitly approved deferral, or documented out-of-scope decision. In autonomous mode, a manual check needs a reproducible procedure and captured result, or an approved external evidence link;
- proposed commit message.

Do not mark a phase ready for approval until its required checks pass, every in-scope traceability ID has exit-checklist evidence, and no unresolved must-fix finding remains. In supervised mode, wait for explicit phase approval. In autonomous mode, require the phase approval and commit action to be covered by the authorization record. Before committing, inspect the staged diff and confirm it contains only phase-owned hunks; stage explicit paths or hunks only, never use `git commit -a`, and stop for user direction if unrelated and phase work in a file cannot be separated. Then commit only the phase's files. Confirm the commit succeeded before focusing on the next phase. Pushing is outside this workflow unless separately added and explicitly authorized. Never rewrite shared history or use destructive Git commands unless the user explicitly requests them.

## 7. Finish the project

Continue the same cycle until every phase is implemented, independently reviewed as required by its risk profile, approved, and committed. Finish with a release summary mapping every traceability ID to its phase, status, exit-checklist evidence, review outcome, and commit. Clearly list unresolved risks, deferred requirements, and anything not checked in a production-like environment. In supervised mode, obtain approval of project completion; in autonomous mode, require project-completion authority in the authorization record. Do not report the project fully delivered while any PRD requirement is deferred or not completed.
