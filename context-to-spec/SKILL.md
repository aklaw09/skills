---
name: context-to-spec
description: Turn rough or conflicting product context into a traceable, reviewable product specification for Hammer Time; use before technical design or implementation.
---

# Context to Spec

Convert the supplied product context into one **Hammer Time Specification**. Own the product contract; do not design its technical solution.

Use this skill for rough feature requests, mixed product evidence, or existing PRDs that need to become an approval-ready specification. Do not use it to choose architecture, make an implementation plan, edit code, or change a repository.

## Work from evidence

1. Read all supplied material. Treat the caller's statements and supplied sources as evidence, not approval.
2. Extract the product problem, actors, outcomes, scope, requirements, acceptance expectations, constraints, dependencies, risks, non-goals, and prior decisions.
3. Keep direct evidence distinct from inferences. Label inferences as assumptions; never turn a missing product decision into a technical assumption.
4. Preserve source requirements. Give each distinct item the required stable ID. Do not silently remove, merge, or materially rewrite an item. Record duplicate or overlapping sources in the relevant item's Source field.
5. Identify contradictions and ambiguity that could affect scope, user-visible behavior, acceptance, compliance, data handling, rollout, or an explicit decision.

### Flag weak stories

A story is **weak** when the supplied context does not support its actor, need, or outcome. Never let a weak story appear as a confirmed product fact.

For every `STORY-…`, add one inline traceability marker in its Need or Outcome cell without changing the required table columns:

- `[Evidence: <source>]` when the source supports the story;
- `[Assumption: RSK-…]` only when it is explicitly an assumption; or
- `[Needs evidence: UNK-…]` when the story is a plausible proposal but lacks support.

For each `[Needs evidence: UNK-…]`, ask the user to provide evidence or explicitly drop the story. Do not include that story in the approved scope, acceptance criteria, or Hammer Time handoff until the user supplies evidence or drops it. A user-approved assumption may remain only when it has an owner and deadline; otherwise it is blocking.

## Resolve material gaps efficiently

Ask only questions that can materially change the specification or whether it can be approved. Before asking, use the supplied context and clearly labeled non-blocking investigation where appropriate.

When questions are needed:

- group them by the decision to be made;
- say briefly why each decision matters;
- distinguish blocking questions from non-blocking investigation;
- do not suggest an invented answer as fact.

If a blocking decision lacks an answer, make a draft specification that records it as `UNK-…`; do not claim it is ready for approval. A blocking unknown may be deferred only when the user explicitly accepts the assumption or deferral and supplies an owner and deadline.

## Test context completeness before handoff

Do not claim that the context is complete merely because no questions are immediately apparent. Completeness is a bounded, evidence-based review: the skill cannot prove that no undiscovered edge case exists.

For every story and requirement, make a coverage pass over the categories that could change the product contract:

- main outcome and alternate paths;
- entry, exit, cancellation, retry, error, and recovery states;
- permissions, roles, ownership, and consent;
- empty, first-use, invalid, duplicate, boundary, and partial-data states;
- lifecycle events such as creation, update, retention, export, deletion, and expiration;
- supported clients, accessibility, localization, compatibility, integrations, and notifications;
- rollout, migration, rollback, support, and operational ownership when relevant.

For each applicable category, record the behavior in **User-visible flows and edge cases**, an acceptance criterion, a constraint, or an unknown. Mark genuinely irrelevant categories as `Not applicable — <reason>`; do not use absence as evidence. If the required behavior, policy, or outcome is not supported by supplied context, create an `UNK-…`. Make it blocking when a reasonable answer would change scope, acceptance, user-visible behavior, compliance, data handling, or rollout.

Before handoff, perform a traceability sweep: every material source statement must map to an identified goal, non-goal, story, requirement, constraint, decision, risk, conflict, or an explicit out-of-scope/deferred disposition. A requirement without a testable acceptance criterion must either receive one or be an `UNK-…` explaining why acceptance cannot yet be judged.

### Independent completeness review

After the self-audit, use an independent subagent review when that capability is available and authorized. Give the reviewer the supplied sources and the draft specification. It reports only:

- unsupported or weak stories;
- material evidence/inference mistakes;
- unchecked completeness categories;
- missing traceability links, conflicts, or acceptance criteria; and
- whether the handoff may proceed.

The reviewer must not edit the specification, invent evidence, decide product questions, or convert an unknown into a decision. Reconcile its evidence-grounded findings before the readiness decision. If an independent reviewer is unavailable, complete the self-audit and do not claim independent review occurred.

## Stable IDs and change control

Use readable prefixes: `USR-`, `GOAL-`, `NGL-`, `STORY-`, `REQ-`, `AC-`, `CON-`, `DEC-`, `UNK-`, `RSK-`, and `CNF-`. Number sequentially within each prefix (for example, `REQ-001`).

Keep IDs stable across revisions. If an approved item changes materially, retain its ID and add a version or supersession entry in the decision log. Never reuse an ID. Every acceptance criterion must name its parent story or requirement. Every functional requirement must have a source and priority.

## Produce the specification

When enough information exists to create a useful draft, emit exactly this structure. Do not omit empty sections or tables: write `None known` in their content instead. Use `Draft` unless all quality gates are met; use `Ready for approval` only when no unaccepted blocking unknown remains. Use `Approved` only after the user explicitly approves it.

```md
# <Product or feature name>

## Document control

- Status: Draft | Ready for approval | Approved
- Owner: <person or team, or Unknown>
- Prepared from: <source list>
- Last updated: <date>
- Decision log: <link/inline entries>

## 1. Problem and outcome

### Problem
<What is wrong today, for whom, and why it matters.>

### Target users
| ID | User or actor | Need | Evidence/source |
|---|---|---|---|

### Goals
| ID | Goal | Success measure | Target/baseline | Priority |
|---|---|---|---|---|

### Non-goals
| ID | Explicitly not included | Reason |
|---|---|---|

## 2. Scope and behavior

### Stories
| ID | Actor | Need | Outcome | Priority |
|---|---|---|---|---|

### Functional requirements
| ID | Requirement | Source | Priority | Status |
|---|---|---|---|---|

### Acceptance criteria
| ID | Parent ID | Given / when / then | Verification intent | Status |
|---|---|---|---|---|

### User-visible flows and edge cases
<Main flow, failure states, permissions, empty states, and important boundary cases.>

## 3. Constraints and operating context

| ID | Constraint or dependency | Why it exists | Source | Status |
|---|---|---|---|---|

Cover, when relevant: compatibility, privacy, security, compliance, supported clients, budget, deadlines, integrations, data retention, rollout, rollback, and ownership.

## 4. Open decisions and risks

### Decisions already made
| ID | Decision | Rationale | Source | Date |
|---|---|---|---|---|

### Unknowns
| ID | Question or unknown | Blocking? | Why it matters | Decision owner | Needed by |
|---|---|---|---|---|---|

### Risks and assumptions
| ID | Risk or assumption | Impact if wrong | Mitigation or validation needed | Owner |
|---|---|---|---|---|

### Conflicts in source material
| ID | Conflicting statements | Affected IDs | Resolution/status |
|---|---|---|---|

## 5. Handoff to Hammer Time

- Ready for design review: Yes | No
- Blocking unknown IDs: <IDs or None>
- Requirements needing technical investigation: <IDs or None>
- Explicit deferrals: <IDs, owner, deadline, and approval status>
- Out-of-scope items: <IDs, reason, and approval status>
- Traceability rules: IDs are stable; changed requirements receive a version or supersession entry; no ID may disappear without an approved disposition.

## 6. Approval request

<Short list of what the user is approving and the decisions still required.>
```

For a section with no applicable items, use `None known` rather than deleting it. For empty tabular sections, preserve the table and add a row whose first cell is `None known`.

## Quality gate and handoff boundary

Before marking the document `Ready for approval`, verify:

1. Each functional requirement has a source and priority.
2. Each acceptance criterion has a parent story or requirement.
3. Each constraint, dependency, risk, assumption, and unknown is visible and identified.
4. Each conflict has a recorded status.
5. Every item is in scope, explicitly deferred, or explicitly out of scope.
6. No blocking unknown remains, unless the user explicitly accepted an assumption or deferral with an owner and deadline.
7. No product facts were invented; inferences are labeled.
8. The handoff accurately states whether design review may begin.
9. Each applicable completeness-review category is represented, marked not applicable with a reason, or tracked as an unknown.
10. Every material source statement has a traceability destination or approved disposition.
11. Every story has an inline evidence, assumption, or needs-evidence marker; no unresolved weak story is in approved scope.
12. When an independent completeness review ran, its findings are resolved, recorded as unknowns, or explicitly deferred.

Finish by asking the user to approve the specification or provide corrections. `Ready for approval` is not approval: do not hand off to or invoke `hammer-time` until the user explicitly approves the specification. Do not create a technical plan, write code, or change files unless separately asked. After approval, the approved specification is the input to `hammer-time`, which owns design review, delivery planning, implementation, and verification.
