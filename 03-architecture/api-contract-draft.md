# API contract draft — `/v1`

Base: `https://api.hulakexpress.com/v1`  
Auth header: `Authorization: Bearer <access_token>`  
Content type: `application/json` (multipart for ticket attachment)  
Envelope:

```json
{ "ok": true, "data": {}, "error": null }
{ "ok": false, "data": null, "error": { "code": "UNVERIFIED_LIMIT", "message": "…" } }
```

Access token TTL: 15 minutes. Refresh TTL: 30 days, rotatable.

## Auth

| Method | Path | Auth | Notes |
|---|---|---|---|
| POST | `/auth/register` | public | individual only in this app |
| POST | `/auth/login` | public | returns tokens + user |
| POST | `/auth/refresh` | refresh token | rotate |
| POST | `/auth/logout` | refresh | revoke |
| POST | `/auth/forgot` | public | always 200 |
| POST | `/auth/reset` | public | token + new password |
| POST | `/auth/verify-email` | public | token |
| POST | `/auth/resend-verification` | restricted | |

Login body: `{ "email", "password", "device": { "platform", "name" } }`

## Me

| Method | Path | Notes |
|---|---|---|
| GET | `/me` | profile + flags |
| PATCH | `/me` | name, phone, address, company |
| POST | `/me/password` | current + new |
| GET | `/me/dashboard` | stats + recent + can_create_* |

`can_create_shipment` / `can_create_pickup` computed server-side.

## Shipments

| Method | Path |
|---|---|
| GET | `/shipments?page=&q=` |
| POST | `/shipments` |
| GET | `/shipments/{id}` |
| GET | `/shipments/{id}/timeline` |

POST body matches current form fields. Response includes `tracking_number`.

## Pickups

| Method | Path |
|---|---|
| GET | `/pickups?page=` |
| POST | `/pickups` |
| GET | `/pickups/{id}` |

## Track (public)

| Method | Path |
|---|---|
| GET | `/track/{trackingNumber}` |

Public payload (redacted):

```json
{
  "tracking_number": "HE2026090700123",
  "status": "in_transit",
  "destination_country": "Japan",
  "estimated_delivery": "2026-09-20",
  "timeline": [
    { "at": "2026-09-07T14:01:00+05:45", "status": "pending", "note": "Booking received" }
  ]
}
```

Owner calling `/shipments/{id}` gets full addresses and phones.

## Tickets

| Method | Path |
|---|---|
| GET | `/tickets` |
| POST | `/tickets` | multipart |
| GET | `/tickets/{id}` |

## Devices

| Method | Path |
|---|---|
| PUT | `/me/devices` | `{ platform, push_token }` |
| DELETE | `/me/devices/{token}` |

## Error codes (initial)

`VALIDATION`, `UNAUTHENTICATED`, `FORBIDDEN`, `ADMIN_NOT_ALLOWED`, `BUSINESS_NOT_ALLOWED`, `UNVERIFIED_LIMIT`, `NOT_FOUND`, `CONFLICT`, `RATE_LIMITED`, `UPLOAD_TOO_LARGE`, `UPLOAD_TYPE`.

## Compatibility rule

The mobile app **must not** call `/user/includes/*` or `/user/ajax/*`. If a field is missing, extend `/v1` — do not scrape HTML.
