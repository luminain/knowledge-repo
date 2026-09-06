# Environments, brand, contacts

## Brand

- Legal: Hulak Express Pvt. Ltd. / Hulak Express Nepal Pvt. Ltd. (confirm exact legal name before store listing)
- Product name: Hulak Express
- Site: https://hulakexpress.com
- Logo in current portal: `../img/email_logo.png` and `https://hulakexpress.com/img/email_logo.png`
- Palette: `#1A237E` / `#00ACC1` / `#FF6B35`
- Type on web: Josefin Sans

## Branches (public)

| Branch | Address | Phone | Email |
|---|---|---|---|
| Kathmandu (hub) | Ratopul, Gaushala | 01-4547495 | ktm@hulakexpress.com |
| Pokhara | (see site) | | |
| Narayangadh | Jun Hall Road, Bharatpur, Chitwan | 9802853851 | nar@uhulakexpress.com (confirm spelling on live site) |

## System contacts (from code, not secrets)

| Role | Address |
|---|---|
| Transactional from | noreply@hulakexpress.com |
| CSR BCC | csr@hulakexpress.com |
| SMTP host | mail.hulakexpress.com:587 STARTTLS |

## Time

- Asia/Kathmandu (UTC+0545; no DST)

## Data stores

| Item | Value |
|---|---|
| DB name | hulakexp_frontend |
| DB host | localhost (current portal) |
| App user | hulakexp_user |

Credentials: **not stored here**. See hosting panel / secrets manager after Phase 0 rotation.

## Adjacent systems not in user.zip

- `admin_dashboard.php` (redirect target)
- Marketing site pages: get_quote, shipping_procedure, blogs
- PHPMailer library expected at `user/../PHPMailer`

## Source snapshot

- `artifacts/user.zip` dated in archive entries through 2026-09-03
- Ticket sample image under `user/uploads/tickets/`
