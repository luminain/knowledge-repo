# Feature inventory — web → mobile

Legend: **MVP** = Phase 1, **P1.5** = immediately after store launch, **P2** = next value, **P3** = later, **No** = do not port.

| Capability | Web today | Mobile |
|---|---|---|
| Register (individual / business fields) | Yes | MVP individual only. Business → deep link to web or message |
| Email verification | Yes | MVP |
| Login | Yes (inconsistent) | MVP, unified rules |
| Forgot / reset password | Yes | MVP (in-app request + deep link) |
| Session persist | Cookie, lifetime 0 | MVP refresh tokens |
| Dashboard stats | Yes | MVP |
| Unverified limit banner | Yes | MVP |
| Create shipment | Yes | MVP |
| List shipments | Yes | MVP |
| Shipment detail | Yes | MVP |
| Create pickup | Yes | MVP |
| List / detail pickup | Yes | MVP |
| Track by number | Yes, public JSON | MVP, public + owned |
| Tracking history timeline | Table exists | MVP if rows exist; empty state otherwise |
| Profile / settings | Yes (handler broken) | MVP |
| Change password | Yes | MVP |
| Support ticket + attachment | Yes | MVP (image/pdf, 5 MB) |
| Ticket list / detail | Yes | MVP |
| Email confirmations | Yes | Keep server-side; app does not send mail |
| Push on status change | No | P1.5 |
| Guest tracking without account | Partial (endpoint) | MVP |
| Saved addresses | No | P2 |
| Duplicate last shipment | No | P2 |
| Get quote / rates | Website only | P2 |
| In-app chat with CSR | No | P3 |
| Payments | No | P2/P3 after finance rules exist |
| Multi-language (EN/NP) | No | P2 |
| Admin redirect | Yes | No — if `is_admin`, refuse and send to web |
| Dark / brand polish | Bootstrap dashboard | MVP must feel like a consumer app, not a ported admin theme |

## Navigation model for mobile (proposed)

Bottom tabs (logged in):

1. Home (stats + recent + CTAs)
2. Shipments
3. Track (also reachable logged out)
4. Pickups
5. More (profile, support, about, logout)

Logged out: Track | Login | Register.
