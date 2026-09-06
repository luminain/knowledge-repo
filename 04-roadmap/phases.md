# Roadmap

Detailed activities, owners, and exit checks: [phase-activities.md](phase-activities.md).  
Team-facing architecture: [../03-architecture/architecture-brief.md](../03-architecture/architecture-brief.md).

## Phase 0 — Foundations (now)

- Stand up this knowledge repository (done 2026-09-07)
- Confirm live schema (`SHOW CREATE TABLE`) and attach to `02-domain/data-model.md`
- Rotate leaked secrets; move config to env
- Decide ADRs 0001–0003
- Unique indexes on tracking / pickup / ticket numbers
- Choose API host and app IDs
- Design screens at fidelity sufficient for implementation (not a 40-screen marketing deck)

Exit: signed ADRs, clean secrets, schema confirmed.

## Phase 1 — Customer parity on mobile

- `/v1` auth + resources
- Flutter (or RN) app feature-complete per `mvp-scope.md`
- Web portal left running; optional later cut-over of dashboard XHR to `/v1`

Exit: closed test on Play + TestFlight with real customers.

## Phase 1.5 — Notify

- device token API
- worker on `tracking_history` insert and ticket status change
- in-app notification inbox (optional; OS notifications are enough)

Exit: status change reaches the phone in < 60s in staging.

## Phase 2 — Reduce typing, add money conversation

- Saved addresses + “ship again”
- Guest-to-account claim of a tracking number
- Quote request from app (wrap existing website form first, native rates later)
- EN/NP language
- Account deletion
- Basic payments only after finance defines what is collected (deposit vs full vs on-delivery)

## Phase 3 — Platform

- Migrate web dashboard onto `/v1` and delete `includes/process_*.php`
- Business app or role-aware workspace
- Rider / pickup-agent app if ops wants barcode scan
- WhatsApp deep links and share card
- SLA dashboard for CSR

## Sequencing rule

Never start Phase 2 features that require a second source of truth. If quotes are still “email us”, the app should collect a quote *request*, not invent a tariff engine.
