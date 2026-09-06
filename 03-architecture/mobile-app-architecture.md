# Mobile app architecture

## Platform

See [ADR-0001](../05-decisions/ADR-0001-mobile-platform.md). Default proposal: **Flutter** (one team, Android-first Nepal market, good store story). React Native is the alternate if the team is already TS-heavy.

## Client layers

```
presentation  ->  feature modules (auth, home, shipments, pickups, track, support, profile)
     |
application   ->  controllers / blocs / riverpod notifiers
     |
domain        ->  entities + repository interfaces (no Dio, no widgets)
     |
data          ->  REST client, DTO, secure storage, local cache
```

Rules:

- Features cannot import each other’s widgets.
- All HTTP behind a single `ApiClient` (base URL, auth header, 401 → refresh, 426 → force update).
- Tokens in Keychain / EncryptedSharedPreferences. Never in SharedPreferences plain text.
- Offline: cache last dashboard + lists. Creates require network. Public track may cache last result per number.

## Navigation

- `AuthGraph`: splash, login, register, forgot, pending-verify
- `MainGraph`: 5 tabs
- `GuestGraph`: track only
- Deep links:
  - `https://hulakexpress.com/track/{trackingNumber}`
  - `https://hulakexpress.com/verify?token=`
  - `https://hulakexpress.com/reset?token=`

## State that is global

- session (access + refresh + user snapshot)
- connectivity
- pending-verify flag
- push permission

Everything else is feature-local.

## Notifications

- Register FCM/APNs token after login.
- Topics are unnecessary; address the user id on the server.
- Types: `shipment_status`, `pickup_status`, `ticket_update`, `system`.
- Tapping a push opens the matching detail screen.

## Observability

- Crashlytics / Crash reporting.
- Non-PII analytics: screen views, create success/fail, track search.
- Do not log tokens, passwords, or full addresses.

## Theming

Reuse brand tokens from the web CSS:

- Primary indigo `#1A237E`
- Secondary cyan `#00ACC1`
- Accent `#FF6B35`
- Font: Josefin Sans if licensed for apps; otherwise a close geometric sans with Nepali script coverage (for Phase 2 NP).

The app must look like a consumer courier product (large tracking CTA, status timeline), not a Bootstrap admin skin.

## App IDs (proposed)

- Android: `com.hulakexpress.user`
- iOS: `com.hulakexpress.user`
- Display name: `Hulak Express`

## Store constraints

- Privacy policy URL required (tracking is public; say so).
- Account deletion path required by stores → add `DELETE /v1/me` or a request-deletion ticket in P1.5.
- Location permission: **not** needed in MVP. Do not request GPS “just in case”.
