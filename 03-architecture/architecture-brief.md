# Hulak Express User platform — architecture brief

**Status:** Working baseline (2026-09-07). Team sign-off still required on ADRs 0001–0003.  
**Audience:** Product, mobile, backend, ops, CSR leads.  
**Rule:** If this page and the code disagree, the code is wrong until this page is updated.

---

## 1. One sentence

We keep the existing customer database and operational tools, put a single HTTPS API in front of them, and build one mobile app that talks only to that API.

## 2. Why this shape

Today customers use a PHP website. That website draws HTML and uses login cookies. A phone app cannot use that safely.

We will **not**:

- wrap the website in a WebView and call it an app
- create a second database for mobile
- put pricing, customs, or warehouse logic in the phone
- let the app call old files such as `includes/process_shipment.php`

We will:

- keep MySQL `hulakexp_frontend` as the system of record
- keep admin/CSR tools as the writers of shipment status
- keep email as it works today
- add a versioned API (`/v1`) that both the future web dashboard and the app use

## 3. Picture

```
Customer phone (iOS / Android)
        |
        |  HTTPS + access token
        v
api.hulakexpress.com  /v1
        |
        +-- Auth (register, login, verify, reset, refresh)
        +-- Me / dashboard
        +-- Shipments
        +-- Pickups
        +-- Track (public, limited fields)
        +-- Tickets
        |
        v
MySQL  (users, shipments, pickup_requests,
        support_tickets, tracking_history,
        + new token / device tables)

Side paths (after a successful save):
        +-- Email (existing SMTP, CSR BCC)
        +-- Push  (Phase 1.5, FCM / APNs)

Not in this picture (on purpose):
        marketing site
        admin dashboard
        quote page
        rider / warehouse tools
```

The phone is a **client**. The API is the **rule owner**. Admin tools are the **status owner**.

## 4. Systems and what each is allowed to do

| System | May create bookings? | May change status? | May see full addresses? |
|---|---|---|---|
| User mobile app | Yes, via API | No | Only the logged-in owner |
| User web dashboard | Yes (today via PHP; later via API) | No | Owner only |
| Public track | No | No | No — status story only |
| Admin / CSR tools | Ops policy | Yes | Yes |
| Marketing site | No | No | No |

## 5. API — the contract everyone shares

Base URL (production): `https://api.hulakexpress.com/v1`  
Staging: `https://api-staging.hulakexpress.com/v1`

Every response looks like:

- success: `{ "ok": true, "data": { … } }`
- failure: `{ "ok": false, "error": { "code": "UNVERIFIED_LIMIT", "message": "…" } }`

Login returns two tokens:

- **Access token** — short life (15 minutes). Sent on every request.
- **Refresh token** — long life (30 days). Used only to get a new access token. Stored on the device in the secure store. Rotated every time it is used.

The app never invents tracking numbers, pickup numbers, or ticket numbers. The server creates them.

Public track (`GET /v1/track/HE…`) returns status, destination country, ETA, and timeline. It does **not** return sender phone or street address.

Full route list: [api-contract-draft.md](api-contract-draft.md).

## 6. Business rules that live in the API (not in the app)

1. This product is for **individual** accounts. Business accounts get a clear error and a link to the business portal.
2. Admins do not use this app.
3. Unverified users may log in, but may create only **one** shipment and **one** pickup in their lifetime until they verify email.
4. New shipment status is `pending`. New pickup status is `pending`. New ticket status is `open`.
5. Pickup date cannot be in the past (Nepal calendar date).
6. Password minimum is **8** characters everywhere (we unify the old 6 vs 8 split).
7. Email is unique.
8. Booking succeeds even if email sending fails; email is retried. The customer still sees the tracking / pickup number.

If a screen in the app needs a new rule, add it here and in the API. Do not hide it in a widget.

## 7. Mobile app — how it is built

One codebase, two stores (proposed: Flutter; change only by accepting a different ADR).

Layers inside the app:

1. **Screens** — what the user sees
2. **Feature logic** — loading, forms, navigation
3. **Domain** — Shipment, Pickup, Ticket, User (no HTTP here)
4. **Data** — REST client, token storage, list cache

Tabs when logged in: Home · Shipments · Track · Pickups · More  
When logged out: Track · Login · Register

Offline: the last dashboard and lists may be shown. Creating a shipment or pickup requires a network. Track may show the last result for that number.

The app does not request GPS in Phase 1.

Application IDs (proposed): `com.hulakexpress.user`  
Display name: Hulak Express  
Brand colours: indigo `#1A237E`, cyan `#00ACC1`, accent `#FF6B35`.

## 8. Data we add (and data we do not copy)

Keep existing tables. Add only what mobile requires:

- refresh-token table (hashed tokens, device id, expiry)
- device-push-token table (Phase 1.5)
- later: saved addresses (Phase 2)

Do **not** duplicate `users` or `shipments` in another database.

Phase 0 also adds unique indexes on `tracking_number`, `pickup_number`, `ticket_number`, and `users.email` if they are missing.

## 9. Environments

| Name | Who uses it | API | Database |
|---|---|---|---|
| Local | Developers | localhost | Docker MySQL + fake mail |
| Staging | QA + internal testers | api-staging.hulakexpress.com | Copy of schema, not live customers |
| Production | Customers | api.hulakexpress.com | Current live DB |

Debug builds default to staging. Production URL is a build flavor, not a hardcoded leftover.

## 10. Security baseline (non-negotiable)

- Secrets live in the host environment, not in git.
- HTTPS only. Verification and reset links use a configured public URL, never `http://` + host header.
- Rate-limit login and forgot-password.
- Password change logs the user out of every device.
- Ticket files are not publicly listable.
- Release builds do not log tokens or addresses.

## 11. What “final” means for this document

Final enough to build Phase 0 and Phase 1. Not final forever.

Still open (need a named owner to accept this week):

- Flutter vs React Native (ADR-0001)
- PHP `/v1` adapter vs Laravel/Node service (ADR-0002 implementation detail)
- Exact legal name for the store listing
- Confirm live columns on `tracking_history`

Everything else in this brief is the default. Changing it requires an ADR and an update to this page.
