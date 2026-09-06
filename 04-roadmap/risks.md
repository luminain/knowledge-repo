# Risks

| ID | Risk | Impact | Mitigation |
|---|---|---|---|
| R1 | Treating old PHP JSON files as the mobile API | Frozen bugs, unshippable auth | ADR-0002: `/v1` only |
| R2 | Secrets already in the zip / git history | Account takeover, mail abuse | Rotate immediately; restrict repo access |
| R3 | Schema guess is wrong | Rework mid-sprint | Dump live DDL in Phase 0 |
| R4 | Unverified-limit race | Abuse of free bookings | Transactional check |
| R5 | Public tracker PII | Privacy / store rejection | Redact in `/v1/track` |
| R6 | Two login implementations | “Works on web, fails on app” | One Login use-case |
| R7 | Email deliverability | Users never verify | Keep current SMTP; monitor bounces |
| R8 | Ops still update status only in admin UI | App looks stale | Confirm admin writes `tracking_history`; if not, add it |
| R9 | Team builds a second database for mobile | Split brain | Forbidden |
| R10 | Scope creep (payments, live map, business portal) | Missed store date | MVP doc is the contract |
| R11 | Store rejection (deletion, privacy) | Launch slip | Policy + delete path in 1.5 |
| R12 | `update_settings.php` already broken | Profile dirty data | Fix only on `/v1`, do not patch blindly |
| R13 | Small team, two platforms | Delay | One codebase (ADR-0001) |
| R14 | Customers live on WhatsApp, not app stores | Low install | Shareable track link + push value |
