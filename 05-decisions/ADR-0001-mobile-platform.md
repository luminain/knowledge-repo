# ADR-0001 — Mobile platform

- Status: Proposed
- Date: 2026-09-07
- Deciders: pending (product + tech lead)

## Context

We need Android and iOS for individual customers. The existing team is PHP/web. Nepal install base is Android-heavy; iOS is required for diaspora and store completeness. Team size does not justify two native squads.

## Decision (proposed)

Build the User app in **Flutter**.

Reasons:

- One codebase, one hire profile
- Fast form-heavy UI (our app is forms + lists + a timeline)
- Predictable release story for Play + App Store
- Enough performance; we are not building a 3D map in MVP

## Consequences

- Design system implemented in Dart widgets, not React.
- Need Flutter CI (Codemagic, GitHub Actions, or equivalent).
- Web dashboard stays PHP; we do not attempt Flutter web as a replacement.

## Alternatives considered

| Option | Why not (unless team composition changes) |
|---|---|
| Two native apps | Double cost, no staff |
| React Native / Expo | Fine if the only mobile engineer is TS-native; accept that instead and update this ADR |
| PWA / TWA wrapping dashboard.php | Session cookies, Bootstrap UI, no push quality, store skepticism |
| Kotlin Multiplatform | Overkill for this product and team |
