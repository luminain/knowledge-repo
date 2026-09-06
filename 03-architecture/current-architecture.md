# Current architecture (as-is)

```
[Browser]
    |  HTML forms + fetch/XHR + PHP session cookie
    v
[Apache/Nginx + PHP user portal]
    |-- index.php, dashboard.php, verify_*, login.php
    |-- includes/process_*.php  (JSON, session required)
    |-- ajax/get_*.php          (JSON, session required)
    |-- includes/track_shipment.php (JSON, public)
    |-- PHPMailer --> mail.hulakexpress.com:587 TLS
    v
[MySQL hulakexp_frontend]
    users, shipments, pickup_requests, support_tickets, tracking_history

[CSR inbox csr@hulakexpress.com]  <-- BCC on create events

[Admin dashboard]  <-- referenced, source not in user.zip
[Marketing site hulakexpress.com] <-- separate
```

## Characteristics

- Monolith, server-rendered dashboard with AJAX islands.
- Authorization = “has a session”. No roles inside the user app besides `is_admin` bounce.
- Business rules duplicated across dashboard.php, refresh_dashboard.php, process_*.php.
- Files for tickets stored on local disk.
- No queue, no job runner, no webhook story, no API versioning, no HTTPS-enforced cookies.

## Why this cannot be the mobile backend as-is

1. Session cookies + SameSite=Strict on a web origin will not survive a native WebView/API client cleanly.
2. Endpoints are file paths, not a stable contract.
3. Responses are inconsistent and sometimes leak PII.
4. CSRF, rate limits, and device identity are absent.
5. A React Native / Flutter app talking to these PHP files would freeze today’s bugs into a second client.

## What we keep

- The MySQL database and the domain records already in it.
- Password hashes (they are portable).
- Email templates and CSR BCC behaviour.
- Tracking number / pickup number semantics.
- The marketing site and admin tools as sibling systems.
