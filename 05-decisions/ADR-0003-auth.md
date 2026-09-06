# ADR-0003 — Authentication

- Status: Proposed
- Date: 2026-09-07
- Deciders: pending

## Context

Web uses PHP sessions with inconsistent checks (`index.php` vs `login.php`). Native apps need durable login without a cookie jar tied to a web origin.

## Decision (proposed)

- Access JWT, 15 minutes, claims: `sub` (user id), `ver` (email_verified), `typ` (individual), `adm` (must be 0).
- Refresh token, 30 days, opaque, hashed in DB, rotated on every refresh, device-bound.
- Unverified users receive tokens with `ver=0`. API enforces create limits using that plus DB truth.
- Admin and business logins are rejected on this API with explicit error codes.
- Password reset and email verify stay token-in-email + deep link.
- Web can keep sessions until Phase 3. Do not share the session cookie with the app.

## Consequences

- New tables for refresh tokens and devices.
- Password change revokes all refresh tokens.
- App must implement refresh-on-401 once, centrally.

## Alternatives considered

| Option | Why not |
|---|---|
| Session cookie in a WebView | Fragile, poor UX, SameSite issues |
| Long-lived JWT only | Cannot revoke |
| OTP-only login | Email OTP later as extra, not instead of password (users already have passwords) |
| OAuth social login | Not needed for MVP |
