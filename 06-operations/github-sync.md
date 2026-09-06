# GitHub remotes and automatic push

## Review (2026-09-07, second pass)

Connected GitHub account: **luminain** (https://github.com/luminain).

| Remote | URL | Visibility | Access |
|---|---|---|
| Requested target | https://github.com/urihsus/knowledge-repo | — | **404** from this integration. Repo does not exist, or it is private and the app cannot see it. |
| `urihsus` public repos that do exist | `hulak-user-app`, `hulak-user-api` | Public | read |
| Live sync this session | https://github.com/luminain/knowledge-repo | Public | admin + write |
| Earlier private copy | https://github.com/luminain/hulak-express-knowledge-repo | Private | admin + write |

`urihsus` is a **user**, not an organization. This connector cannot create `urihsus/knowledge-repo` on someone else’s account.

To make `urihsus/knowledge-repo` the canonical remote:

1. On the `urihsus` account, create the empty repo `knowledge-repo`.
2. Add `luminain` as a collaborator with write, **and** grant the same GitHub App used by this Grok connector **Contents: Read and write** on that repo.
3. Ask Grok to sync again.

Until then, the project folder syncs to https://github.com/luminain/knowledge-repo.

Do not put secrets, `.env`, SMTP passwords, `user.zip`, or ticket uploads on any of these remotes.
