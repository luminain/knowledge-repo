# Product vision — Hulak Express User app

## Company context

Hulak Express (Hulak Express Pvt. Ltd. / Hulak Express Nepal) is an international cargo and courier operator based in Nepal. Public positioning:

- Ship from Nepal to 150+ countries
- Door-to-door courier, air cargo, sea freight (FCL/LCL)
- Free pickup above a minimum weight (marketing claim; not encoded in the current user portal)
- Branches: Kathmandu (Ratopul, Gaushala — primary hub), Pokhara, Narayangadh (Bharatpur)
- Typical goods: food items, clothes, handicrafts, documents, household and commercial cargo
- Brand site: https://hulakexpress.com

The company already runs a **web user portal**. Operations, quoting, and customer communication are still largely web- and email-centric. Customers in this market live on mobile.

## Problem

Customers book pickups, create shipments, and track parcels on a desktop-oriented PHP dashboard. That creates friction for:

- diaspora senders booking from a phone while packing
- receivers asking “where is my box?”
- repeat customers who just need status, not a full admin UI

There is no first-party User mobile app. Session cookies and page-rendered PHP cannot be consumed cleanly by a native client.

## Opportunity

A User app that is a thin, reliable client on top of a new API layer:

1. Same account as the web portal (one identity).
2. Faster booking and tracking than the current dashboard.
3. Push notifications when status changes (the single highest-ROI mobile feature; the web app has no equivalent).
4. A foundation for later quote, payment, and saved-address flows without rewriting the operational core.

## Product principle

**Do not rebuild the logistics company in the phone.** Rebuild the *customer surface*. The first release must reproduce the existing user journeys with a proper API and mobile UX. New capabilities (push, quotes, payments, live map) come after that surface is stable.

## Goals (12 months)

| Goal | Measure |
|---|---|
| Parity on core journeys | Login, register, verify, create shipment, create pickup, list + detail, track, ticket, profile |
| Mobile is a first-class channel | ≥ 40% of new shipments from verified mobile users originate in the app (once launched) |
| Reduce “where is my shipment?” tickets | Status push + in-app timeline cuts tracking-related tickets |
| One backend for web and mobile | No second database, no forked business rules |
| Continuity | Any new engineer can onboard from this knowledge repo in < 1 day |

## Non-goals (explicit)

- Admin / CSR / warehouse / rider apps (separate products)
- Replacing the public marketing site
- Full WMS, customs filing, or airline integration in MVP
- In-app payment collection in Phase 1
- Live GPS of the parcel in Phase 1 (status timeline only)
- Business-account portal features (this app is the **individual** customer surface; business remains a separate portal until an ADR says otherwise)

## Success for Phase 1 (MVP)

A verified individual customer can, on Android and iOS:

- create an account and verify email
- sign in and stay signed in
- create one shipment and one pickup
- see lists and details
- track by `HE…` number (own shipments, and public track by number)
- open a support ticket
- receive email confirmations identical in meaning to today

Push notifications are Phase 1.5 if token infrastructure slips; they must not block the store listing if email already works.
