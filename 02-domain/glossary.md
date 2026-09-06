# Glossary

| Term | Meaning |
|---|---|
| User / Customer | Individual account holder on the user portal / app |
| Account type | `individual` or `business`. This app serves `individual` |
| Verified user | `users.email_verified = 1` |
| Shipment | A booked outbound consignment owned by a user |
| Tracking number | Public identifier, format `HE` + `YYYYMMDD` + 5 digits |
| Pickup request | Scheduled collection from a customer address |
| Pickup number | `PICKUP` + `YYYYMMDD` + 5 digits |
| Support ticket | Customer-initiated CSR thread |
| Ticket number | `TICKET-` + uppercase `uniqid()` |
| Tracking history | Append-only status events on a shipment |
| CSR | Customer service; mailbox `csr@hulakexpress.com` |
| Service type | Customer-selected service; web default `standard` |
| Package type | Web default `parcel` |
| Portal | The existing PHP web app in `user.zip` |
| User API | The new JSON API both web and mobile will call |
| HE | Prefix used in tracking numbers (Hulak Express) |
