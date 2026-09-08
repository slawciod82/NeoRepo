# Context Architecture — Rulebook v2

Status: Active

## Purpose

Keep discovery work reversible and focused by separating active task instructions from the broader body of product and architecture knowledge.

## Rules

1. `AGENTS.md` is the entry contract.
2. GitHub current state is authoritative; read current HEAD and relevant documents before editing.
3. Discovery documents are loaded only when the task touches their decision area.
4. Drafts and superseded ideas are searchable context, not active instruction.
5. Default context budget: entry contract + task + current HEAD + directly relevant product/architecture docs.
6. Do not convert assumptions into final architecture without an explicit decision.
7. Prefer ADR/decision records for durable architecture choices.
8. MVP scope and end-to-end validation outrank speculative platform completeness.
9. Handoffs are task-scoped compilations, not discovery-history summaries.
10. New rules pass the Rulebook Growth Gate before becoming always-active.

## Sprint and task context

### Sprint Context Rule

A sprint document is a planning source, not mandatory full execution context. For implementation or discovery execution, load only the sprint goal, scope, acceptance criteria, dependencies, risks or blockers that materially affect the current task. Read the full sprint only when the task is ambiguous, changes sprint scope, depends on another sprint item, or requires sprint-level planning or review.

### Task as Execution Unit

The task is the primary unit of execution context and handoff. A task should define, as needed: goal, business reason, in-scope work, out-of-scope work, acceptance criteria, relevant documents/modules, activated rules/ADRs, dependencies, task-scoped assumptions and verification/decision output. Small discovery tasks may keep this compact. Execution context is compiled around the current task rather than around the whole sprint.

Task lifecycle should normally follow `BACKLOG -> READY -> IN_PROGRESS -> VERIFY -> DONE`, with `BLOCKED` when needed. `DONE` requires the applicable acceptance/verification or explicit decision gate; produced documentation or code alone is not completion.

### Sprint Summary = Historical Context

Sprint summaries record what actually happened and are historical evidence, not active instructions for later tasks. They are OFF by default. Load a previous sprint summary only when the current task needs evidence about what was decided or implemented, why a decision was made, a known limitation, or a dependency on prior work.

Task-scoped assumptions expire with the task unless they are deliberately promoted to a durable invariant, ADR/decision or rule through the normal Rulebook Growth Gate.

## Rulebook Growth Gate

Classify a new rule as Core invariant, domain/module rule, project invariant, ADR/product decision, verification/security check, or historical information. Only rare Core invariants are always loaded.

## Priority

P0 safety/data integrity > P1 product/architecture invariants > P2 project rules > P3 task-specific preferences > P4 style/convenience.

## NeoRepo activation examples

- MVP/product boundary -> product vision + MVP roadmap
- storage/provenance/immutability -> immutable object storage decision
- architecture boundaries -> architecture draft
- future ingestion/security -> only the relevant source/storage/security documents

Do not preload the entire discovery corpus.
