# Hulak Express — Project Knowledge Repository

Central source of truth for the **Hulak Express User** product line: the existing web portal and the planned native/cross-platform mobile application.

Any engineer, PM, designer, or stakeholder should be able to join mid-stream and reconstruct:

- what the business does
- what the current system actually does (not what we wish it did)
- the target mobile architecture
- the roadmap, open decisions, and constraints

## How to use this repo

1. Start at [INDEX.md](INDEX.md).
2. Read product + domain before architecture.
3. Treat `05-decisions/` as binding unless a newer ADR supersedes it.
4. When you change a foundational fact, update the relevant doc in the same PR/commit. Do not leave tribal knowledge in chat.

## What this repo is not

- Not a dump of source code (the web snapshot lives in `user.zip`).
- Not a place for secrets. Credentials stay in a secrets manager / env vars. See [03-architecture/security.md](03-architecture/security.md).
- Not the admin, CSR, or rider apps. Those are adjacent systems and are only referenced where they affect the User app.

## Ownership

| Area | Default owner |
|---|---|
| Product scope & roadmap | Product |
| Domain model & business rules | Product + Backend |
| Target architecture & ADRs | Tech lead |
| API contracts | Backend + Mobile |
| Security & secrets | Tech lead |

Last initialized: 2026-09-07.
