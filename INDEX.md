# Knowledge map

## 01 — Product

| Doc | Why it exists |
|---|---|
| [01-product/vision.md](01-product/vision.md) | Why a User mobile app, success metrics, non-goals |
| [01-product/current-web-platform.md](01-product/current-web-platform.md) | Honest inventory of the live PHP portal |
| [01-product/user-personas.md](01-product/user-personas.md) | Who we are building for |
| [01-product/feature-inventory.md](01-product/feature-inventory.md) | Web features → mobile MVP / later / drop |

## 02 — Domain

| Doc | Why it exists |
|---|---|
| [02-domain/glossary.md](02-domain/glossary.md) | Shared language |
| [02-domain/business-rules.md](02-domain/business-rules.md) | Rules the app must enforce |
| [02-domain/data-model.md](02-domain/data-model.md) | Tables and fields inferred from the current portal |

## 03 — Architecture

| Doc | Why it exists |
|---|---|
| [03-architecture/current-architecture.md](03-architecture/current-architecture.md) | As-is system |
| [03-architecture/architecture-brief.md](03-architecture/architecture-brief.md) | Team-facing baseline — read this first |
| [03-architecture/target-architecture.md](03-architecture/target-architecture.md) | To-be system for web + mobile |
| [03-architecture/mobile-app-architecture.md](03-architecture/mobile-app-architecture.md) | Client architecture |
| [03-architecture/api-contract-draft.md](03-architecture/api-contract-draft.md) | First-cut REST surface |
| [03-architecture/security.md](03-architecture/security.md) | Auth, secrets, abuse |

## 04 — Roadmap

| Doc | Why it exists |
|---|---|
| [04-roadmap/mvp-scope.md](04-roadmap/mvp-scope.md) | What ships first |
| [04-roadmap/phases.md](04-roadmap/phases.md) | Phased plan |
| [04-roadmap/phase-activities.md](04-roadmap/phase-activities.md) | Sub-phases, owners, done-when |
| [04-roadmap/risks.md](04-roadmap/risks.md) | Risks and mitigations |

## 05 — Decisions

| Doc | Status |
|---|---|
| [05-decisions/ADR-0001-mobile-platform.md](05-decisions/ADR-0001-mobile-platform.md) | Proposed |
| [05-decisions/ADR-0002-api-strategy.md](05-decisions/ADR-0002-api-strategy.md) | Proposed |
| [05-decisions/ADR-0003-auth.md](05-decisions/ADR-0003-auth.md) | Proposed |
| [05-decisions/ADR-template.md](05-decisions/ADR-template.md) | Template |

## 07 — Beginner build guide

| Doc | Why it exists |
|---|---|
| [07-beginner-guide/developing-the-app.md](07-beginner-guide/developing-the-app.md) | Zero-to-closed-test steps for someone new to app development |
| [07-beginner-guide/github-repos.md](07-beginner-guide/github-repos.md) | Create GitHub repos and push source |

## 06 — Operations

| Doc | Why it exists |
|---|---|
| [06-operations/environments-and-contacts.md](06-operations/environments-and-contacts.md) | Branches, emails, timezone, brand |
| [06-operations/github-sync.md](06-operations/github-sync.md) | Remotes + automatic push to GitHub |

## Source snapshot

- `artifacts/user.zip` — PHP user portal as of early September 2026.
- Key paths inside the zip: `user/index.php`, `user/dashboard.php`, `user/includes/*`, `user/ajax/*`, `user/config/database.php`.
