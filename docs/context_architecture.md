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
