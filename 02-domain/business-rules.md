# Business rules (encoded + implied)

Rules below are extracted from the current PHP portal. If operations disagree, change the rule here *and* in code. Do not special-case in the mobile client.

## Identity

1. Email is the unique login identifier.
2. Passwords are stored with `password_hash` (PASSWORD_DEFAULT).
3. Registration password minimum is **8** characters (`index.php`). Settings change minimum is **6** (`update_settings.php`). **Unify to 8** for API and app.
4. Business registration requires `company`. Individual does not.
5. This surface accepts **individual** logins only.
6. `is_admin = 1` must not use the User app. Send them to the admin web tool.
7. Email verification token lives 24 hours.
8. Password reset token lives 1 hour (`reset_token`, `reset_expires`).
9. Login on the main portal currently *requires* verification. Dashboard still allows an unverified session if obtained another way. **API rule:** unverified users may authenticate with a restricted token scope (`unverified`) so they can resend verification and see the limit banner. They cannot exceed create limits.

## Create limits

10. If `email_verified = 0`, user may create at most **1 shipment** and **1 pickup request** (lifetime count, not rolling).
11. Limit check must be transactional (SELECT … FOR UPDATE or equivalent) to close the current race.

## Shipments

12. Required fields: sender name/address/phone, receiver name/address/phone, destination country, weight, service type, estimated delivery.
13. Optional: package type (default `parcel`), dimensions, contents, insurance amount (default 0), notes.
14. New shipments start at status `pending`.
15. Tracking number generated server-side. Client never invents it.
16. Generation must retry on unique constraint collision (today it does not).
17. Active statuses for dashboard “in progress”: `pending`, `picked_up`, `in_transit`, `out_for_delivery`.
18. Delivered status: `delivered`.
19. Status transitions are **ops-owned**. The User app is read-only on status.
20. Creating a shipment sends confirmation email to the user and BCC to CSR.

## Pickups

21. Required: pickup address, date, time.
22. Pickup date cannot be in the past (date only; timezone Asia/Kathmandu).
23. Optional: package count (default 1), approximate weight (default 0), package type (default `parcel`), special instructions.
24. New pickups start at `pending`.
25. Pickup number generated server-side.
26. Creating a pickup sends confirmation email to the user (and CSR via the mail helper BCC pattern).

## Tracking

27. Track-by-number is public.
28. Public track responses must be **minimized**: tracking number, status, estimated delivery, history events, destination country. Do **not** return raw sender phone/address to anonymous callers (current PHP does). Owner token may see full PII.
29. History ordered newest first.

## Support

30. Required: subject, priority, message.
31. Optional: `shipment_id`, attachment.
32. Attachment max 5 MB. Types: jpg, jpeg, png, gif, pdf, doc, docx, txt.
33. New tickets start `open`. Active dashboard count = `open` + `in_progress`.
34. Ticket create sends confirmation email.

## Profile

35. Full name and email required.
36. Email change must remain unique.
37. Password change requires current password + matching new/confirm.

## Notifications

38. Email is the current system of record for customer notifications.
39. Push is additive: same events as email (shipment created, pickup created, ticket created, **plus** status changes — status change has no email today and is the gap).

## Time and locale

40. All server timestamps: `Asia/Kathmandu`.
41. MVP UI language: English. Copy may later add Nepali; do not hardcode English-only in the API.
