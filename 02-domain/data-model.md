# Data model (inferred from the user portal)

No official schema dump shipped with `user.zip`. The following is reconstructed from SQL in the PHP files. Treat as the working model until a live `SHOW CREATE TABLE` is attached here.

## `users`

| Column | Evidence | Notes |
|---|---|---|
| id | PK used everywhere | |
| full_name | register, session | |
| email | unique login | |
| password | password_hash | |
| phone | register / settings | |
| address | register / settings | |
| account_type | `individual` / `business` | |
| company | business | |
| email_verified | 0/1 | |
| verification_token | register / resend | |
| token_expires | datetime | |
| reset_token | added if missing | |
| reset_expires | datetime | |
| is_admin | 0/1 | |
| last_login | set on successful index.php login | |
| created_at | assumed | confirm on live DB |

## `shipments`

| Column | Evidence |
|---|---|
| id | PK, used as ticket.shipment_id and tracking_history.shipment_id |
| user_id | FK users |
| tracking_number | `HEYYYYMMDD#####` |
| sender_name, sender_address, sender_phone | required |
| receiver_name, receiver_address, receiver_phone | required |
| destination_country | required |
| package_type | default parcel |
| weight | float, required |
| dimensions | optional string |
| contents | optional |
| service_type | default standard |
| insurance_amount | float, default 0 |
| estimated_delivery | required (string/date from form) |
| notes | optional |
| status | pending / picked_up / in_transit / out_for_delivery / delivered |
| created_at | NOW() |

## `pickup_requests`

| Column | Evidence |
|---|---|
| id | PK |
| user_id | FK |
| pickup_number | `PICKUPYYYYMMDD#####` |
| pickup_address | required |
| pickup_date | date |
| pickup_time | time/string |
| package_count | int default 1 |
| approximate_weight | float |
| package_type | default parcel |
| special_instructions | optional |
| status | pending (others used by ops; not enumerated in user code) |
| created_at | NOW() |

## `support_tickets`

| Column | Evidence |
|---|---|
| id | PK |
| ticket_number | `TICKET-…` |
| user_id | FK |
| subject | |
| priority | form-driven; values not constrained in PHP |
| message | |
| attachment | relative path or null |
| shipment_id | nullable |
| status | open, in_progress, (closed implied) |
| created_at, updated_at | NOW() |

## `tracking_history`

| Column | Evidence |
|---|---|
| id | assumed PK |
| shipment_id | FK |
| created_at | order by DESC |
| other columns | **unknown** — likely status, location, notes. Confirm on live DB before locking the API |

## Suggested physical constraints (do this in Phase 0)

```sql
ALTER TABLE shipments ADD UNIQUE KEY uq_shipments_tracking (tracking_number);
ALTER TABLE pickup_requests ADD UNIQUE KEY uq_pickups_number (pickup_number);
ALTER TABLE support_tickets ADD UNIQUE KEY uq_tickets_number (ticket_number);
ALTER TABLE users ADD UNIQUE KEY uq_users_email (email);
```

Plus indexes on `shipments(user_id, created_at)`, `pickup_requests(user_id, created_at)`, `support_tickets(user_id, created_at)`, `tracking_history(shipment_id, created_at)`.

## Tables we will likely add for mobile (do not invent in the PHP pages)

- `oauth_refresh_tokens` or `user_refresh_tokens`
- `device_push_tokens` (user_id, platform, token, last_seen)
- `saved_addresses` (Phase 2)
- `audit_log` (auth + create events)
