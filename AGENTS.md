# NeoRepo — Agent Entry Contract

NeoRepo is currently in draft/discovery. This file is the mandatory entry point for coding/documentation agents and is intentionally small.

## Always-active core

- GitHub is the source of truth. Read current HEAD and current files before editing.
- Do not rely on an old checkout, previous conversation state, or remembered documentation.
- KISS. Prefer one complete end-to-end path over broad speculative architecture.
- Keep discovery decisions reversible unless evidence justifies locking them down.
- Do not turn draft assumptions into final architecture silently.
- Code/documents communicate WHAT; rationale explains WHY and records the decision source where practical.
- Apply the Framework Threshold Rule: shared cross-project components require repeated, stable use cases before extraction.
- Do not claim implementation or tests exist unless they actually do.
- Security Baseline Gate becomes mandatory before real institutional data, pilot, or production.

## Context budget

Default active context is:

1. this file,
2. the current task,
3. current HEAD,
4. only the directly relevant discovery/architecture/product files.

Do not preload every architecture, product and roadmap document. History and superseded drafts are searchable context, not active instructions.

## Rule activation

- product scope / MVP boundary -> `docs/product/vision.md` and `docs/roadmap/mvp_and_post_mvp.md`
- system architecture / ownership boundaries -> `docs/architecture/architecture_draft.md`
- storage, provenance, immutability or derived artifacts -> `docs/architecture/immutable_object_storage.md` plus only directly related architecture docs
- future implementation task -> read the current architecture decision for that component plus the smallest applicable engineering/security rules
- uncertainty -> prefer explicit assumption/decision notes rather than inventing certainty

## NeoRepo-specific invariants

NeoRepo is an institutional repository for recordings and digital materials covering ingestion, verification, processing, metadata, publication, retention and archiving.

It is not a Git repository for large files, generic network share, candidate/participant Document Vault, LMS, or public video platform at MVP stage.

Preserve these boundaries:

- originals and derived artifacts are distinguishable,
- provenance/history of processing remains explicit,
- file integrity is a first-class concern,
- object storage and business metadata have separate responsibilities,
- MVP should validate one complete end-to-end flow,
- post-MVP complexity must not enter MVP without a measurable need.

Because the project is still discovery-first, prefer ADR/decision updates over premature code when the task is architectural.

## Handoff rule

When asked to prepare a handoff, produce a short task-scoped handoff. Include repository/branch, source-of-truth rule, discovery status, current task, only relevant product/architecture documents, unresolved assumptions, and verification/decision output required. Do not dump all discovery history and do not hard-code a HEAD SHA as authoritative.

## Verification/reporting

For documentation/architecture work report changed files, decisions made, assumptions left open, and any scope/MVP impact. For future code work also report commit SHA/current HEAD and tests actually executed or still required.
