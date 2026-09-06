# Target architecture (web + User mobile)

Plain-language baseline for the whole team: [architecture-brief.md](architecture-brief.md).

Strategy: **strangle the PHP pages with a versioned API that owns the rules**. Mobile is a client. The existing dashboard can keep running while we migrate it onto the same API.

```
                    +------------------+
                    |  Marketing site  |
                    +------------------+

[iOS] [Android]                  [Web dashboard]
   \     /                            |
    \   /                             |
 [User App]                      (later: same API)
        \                            /
         \                          /
          v                        v
        +----------------------------+
        |  API gateway / HTTPS       |
        |  api.hulakexpress.com      |
        |  /v1                       |
        +----------------------------+
          |  JWT access + refresh
          |  rate limit, CORS, audit
          v
        +----------------------------+
        |  User Service (API)        |
        |  auth, shipments, pickups, |
        |  tickets, track, profile   |
        +----------------------------+
          |                 |
          v                 v
     [MySQL]         [Mail + Push workers]
 hulakexp_frontend    SMTP existing
                      FCM / APNs

        +----------------------------+
        |  Ops / Admin (unchanged)   |
        |  writes status + history   |
        +----------------------------+
```

## Backend choice (recommended)

Keep PHP in the short term **only as an adapter** if that is what ops can deploy this month. Do not grow more procedural files.

Preferred implementation for `/v1`:

- New `apps/api` service (Laravel or slim PHP 8.3+ with a proper router, **or** Node/Nest if the team is stronger there).
- Same MySQL database.
- One module per bounded context: Auth, Shipments, Pickups, Tracking, Tickets, Users.
- Side effect (mail, push) after commit, via a small queue (Redis + worker) or at least `register_shutdown` / async HTTP. Do not fail the booking if mail fails; log and retry.

If the team must ship in weeks with current hosting: implement `/v1` as a new folder `user/api/v1/` with a single front controller, JWT, and shared domain functions extracted from the process_* files. That is acceptable as Phase 0–1 **if and only if** the mobile app never calls the old `includes/` or `ajax/` paths.

## Shared kernel

All clients (web, mobile) must call the same use-cases:

- `RegisterUser`
- `VerifyEmail`
- `Login`
- `RefreshToken` / `Logout`
- `GetDashboard`
- `CreateShipment` / `ListShipments` / `GetShipment`
- `CreatePickup` / `ListPickups` / `GetPickup`
- `TrackPublic`
- `CreateTicket` / `ListTickets` / `GetTicket`
- `UpdateProfile` / `ChangePassword`

No business rule in the Flutter/RN layer except input UX (empty field, date picker).

## Environments

| Name | App API | Notes |
|---|---|---|
| local | http://localhost:8080 | docker-compose MySQL + mailhog |
| staging | https://api-staging.hulakexpress.com | copy of schema, anonymized data |
| production | https://api.hulakexpress.com | existing DB |

Never point a debug build at production by default.

## Integration map

| System | Direction | Phase |
|---|---|---|
| MySQL | API read/write | 0 |
| SMTP | API → customer + CSR | 1 |
| FCM / APNs | worker → device | 1.5 |
| Admin tool | writes shipment.status + tracking_history | already |
| WhatsApp | out of band (customers share tracking) | deep links P2 |
| Payment PSP | later | 2+ |
| Quote engine | later; currently a website form | 2 |

## Deployment sketch

- API: git → CI (lint, tests, schema check) → staging → prod.
- Mobile: store track (internal → closed test → production).
- Feature flags: `push_enabled`, `guest_track_pii_redacted` (default on).
