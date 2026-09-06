# ADR-0002 — API strategy

- Status: Proposed
- Date: 2026-09-07
- Deciders: pending

## Context

The portal exposes session JSON under `includes/` and `ajax/`. That is not an API. Mobile needs a stable, authenticated, versioned contract. The database and email behaviour must stay.

## Decision (proposed)

Introduce **`/v1` as the only mobile backend**. Extract domain use-cases from the PHP processors. Do not point the app at existing file endpoints.

Implementation may start as a PHP front controller on the current host to ship fast, provided:

- JWT (not session cookies)
- unified validation
- PII-redacted public track
- env-based secrets

A framework upgrade (Laravel or similar) can follow without changing the contract.

## Consequences

- Two surfaces for a while (old pages + `/v1`).
- Web dashboard migration is Phase 3, not a blocker.
- Contract lives in `03-architecture/api-contract-draft.md` and later an OpenAPI file.

## Alternatives considered

| Option | Why not |
|---|---|
| Call existing PHP files from the app | Auth + CSRF + PII + path fragility |
| New Node API + new Mongo | Split brain, extra ops |
| GraphQL from day one | Team overhead; our resources are obvious CRUD |
