# Current web platform (as-is)

Source of truth: `artifacts/user.zip` (user portal PHP app). This is an inventory, not a redesign.

## Stack

| Layer | Reality |
|---|---|
| Language | PHP (procedural, page + include handlers) |
| DB | MySQL via PDO, database name `hulakexp_frontend` |
| Auth | PHP sessions, `password_hash` / `password_verify` |
| UI | Bootstrap 5, DataTables, Select2, Font Awesome 6, Josefin Sans |
| Email | PHPMailer SMTP to `mail.hulakexpress.com` |
| Hosting model | Classic LAMP-style, `.htaccess` present |
| Timezone | `Asia/Kathmandu` |
| Error mode | `display_errors = 1` in `config/database.php` (not production-safe) |

There is **no versioned public API**. JSON endpoints exist under `user/ajax/` and `user/includes/` but they are session-cookie authenticated page helpers, not a mobile contract.

## Screens / modules

| Module | Entry | Notes |
|---|---|---|
| Marketing + auth shell | `index.php` | Login, register, forgot/reset password, verification email send |
| Login POST (legacy) | `login.php` | Duplicate login path; weaker rules than `index.php` |
| Dashboard SPA-ish | `dashboard.php` | One page, section switching via JS (`data-section`) |
| Email verify request | `verify_email.php` | Logged-in resend; demo path stores link in session |
| Confirm verify | `confirm_verify.php` | Token consume |
| Logout | `logout.php` | |
| Errors | `404.php`, `440.php`, `500.php`, `unauthorized.php` | `includes/login.php` just redirects to `440.php` |

Dashboard sections: Dashboard, New Shipment, List Shipments, Pickup Request, Tracking, Settings, Support.

## Write endpoints

| Action | File | Response |
|---|---|---|
| Create shipment | `includes/process_shipment.php` | JSON |
| Create pickup | `includes/process_pickup.php` | JSON |
| Create ticket | `includes/create_support_ticket.php` | JSON + optional file |
| Track | `includes/track_shipment.php` | JSON, **no login required** |
| Update profile | `includes/update_settings.php` | JSON — **requires missing `config.php`** (broken relative include) |

## Read endpoints (`ajax/`)

All expect an active session:

- `refresh_dashboard.php` — stats + flags `can_create_shipment` / `can_create_pickup`
- `get_all_shipments.php`, `get_recent_shipments.php`, `get_shipment_details.php`
- `get_recent_pickups.php`, `get_pickup_details.php`
- `get_support_tickets.php`, `get_recent_tickets.php`, `get_ticket_details.php`

## Auth behaviour (inconsistent — must be unified before mobile)

`index.php` login:

- individual accounts only (`account_type !== 'individual'` is rejected)
- requires `email_verified`
- sets `user_id`, `email`, `full_name`, `logged_in`, `is_admin`, `account_type`
- updates `last_login`

`login.php` login:

- any user by email
- does **not** check verification or account type
- sets `user_id`, `user_name`, `user_email`, `account_type`, `logged_in`

`dashboard.php`:

- requires `$_SESSION['user_id']` only
- redirects `is_admin == 1` to `admin_dashboard.php` (file not in this zip)
- allows unverified users in, with create limits

**Decision for mobile:** follow the stricter product intent (individual + verified to use the app fully; unverified may register and see the limit banner, matching dashboard).

## Email

Transactional mail is first-class today:

- verify email
- password reset
- shipment created (user + BCC CSR)
- pickup created
- ticket created

CSR mailbox: `csr@hulakexpress.com`. From: `noreply@hulakexpress.com`.

## Known defects that the mobile program must not copy

1. Secrets committed in `config/database.php` and `email_helper.php` / `index.php`.
2. HTTP (not HTTPS) verification and reset links built from `$_SERVER['HTTP_HOST']`.
3. Session cookie `secure => false`.
4. `update_settings.php` includes a file that is not in the tree.
5. Two login implementations with different rules.
6. Tracking endpoint returns full shipment row including sender PII to anyone who knows the tracking number.
7. Ticket upload path is relative (`uploads/tickets/`) from `includes/`, while the zip stores files under `user/uploads/tickets/` — path drift risk.
8. Tracking number collision possible (`mt_rand` 1–99999, no uniqueness retry).
9. Unverified-user limit is count-based and raceable (no transaction).
10. `verify_email.php` comments still describe a “demo” session-link path vs the real PHPMailer path in `index.php`.

## What the web app does *not* do

- Rate quotes or pricing
- Payments / COD / invoices
- Saved address book (sender fields are retyped)
- Multi-package line items (pickup has `package_count` only)
- Map / live location
- Push or SMS
- Multi-language UI (English only in code; brand marketing is EN + NP)
- Business-account workflows in this portal
