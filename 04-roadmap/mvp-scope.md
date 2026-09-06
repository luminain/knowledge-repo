# MVP scope (Phase 1)

## In

- Register individual, verify email, login, logout, forgot/reset
- Restricted session for unverified users
- Home stats + recent lists
- Create / list / detail shipment
- Create / list / detail pickup
- Public + owner tracking with timeline
- Profile edit + password change
- Support ticket with optional image
- Email side effects unchanged
- Android + iOS from one codebase
- Staging API + production API
- Force-update header hook
- Knowledge repo kept current

## Out

- Push (Phase 1.5, week after MVP freeze if tokens are late)
- Quotes, payments, saved addresses
- Nepali language pack
- Business accounts
- Admin features
- Live maps / GPS
- Chat
- Redesign of ops/admin tools

## Definition of done

- All `/v1` endpoints above have automated tests for happy path + unverified limit + public PII redaction
- App passes internal QA on a mid-range Android and one iOS device
- Existing web portal still works (we did not break bookings)
- No secrets in git
- Store listing copy + privacy policy drafted
- CSR notified of new channel (same emails, new user-agent on bookings is fine)

## Effort shape (small team, 1 mobile + 1 backend + 0.5 PM/design)

| Workstream | Weeks (calendar) |
|---|---|
| Phase 0 foundations | 2 |
| Phase 1 API + app | 8–10 |
| QA + store | 2 |
| Phase 1.5 push | 2 |

Total to a public store listing: ~12–14 weeks if decisions in `05-decisions/` are accepted in week 1.
