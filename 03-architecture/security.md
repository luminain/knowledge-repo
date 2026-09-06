# Security

## Secrets — act now

The user portal snapshot contains live-looking credentials in source:

- MySQL user/password in `user/config/database.php`
- SMTP password in `user/index.php` and `user/includes/email_helper.php`

**Do not copy those values into this knowledge repo, tickets, or chat logs.**  
Phase 0 work:

1. Rotate DB password and SMTP password.
2. Move to environment variables / hosting panel secrets.
3. Disable `display_errors` in production.
4. Force HTTPS. Set session (and future cookie) `secure=true`.
5. Build verification and reset links with a configured `PUBLIC_BASE_URL`, not `http://` + `HTTP_HOST`.

## Auth for mobile

- Short-lived JWT access + rotating refresh tokens stored hashed at rest.
- Bind refresh tokens to a `device_id`.
- Revoke all refresh tokens on password change.
- Lockout / rate limit login and forgot-password per IP + email.
- Do not put `is_admin` privileges in the User API even if the flag exists.

## Data minimization

Current public tracker returns the full shipment row. That is a PII leak.  
Public `/v1/track/{id}` returns status story only. Full PII only for the owning user.

## Uploads

- Store outside the web root or on object storage.
- Randomize file names (already time-prefixed; keep it).
- Virus scan is nice-to-have; type + size limits are mandatory.
- Serve attachments only to the ticket owner and CSR/admin.

## Mobile client

- Certificate pinning is optional in MVP; do it once the API hostname is stable.
- No debug logs of payloads in release.
- Root/jailbreak detection is optional; not a substitute for server checks.

## Compliance notes (Nepal + stores)

- Publish a privacy policy covering account data, shipment PII, and device tokens.
- Provide a deletion request path before store review.
- Tracking numbers are shared by customers on WhatsApp; design public track accordingly.
