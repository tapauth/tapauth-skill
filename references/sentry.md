# Sentry via TapAuth

## Provider Key

Use `sentry` as the provider name.

## Scope Model: Permission-Based

Sentry uses a `resource:action` permission model. Each resource has `read`, `write`, and `admin` levels, where higher levels include lower ones (e.g., `write` includes `read`).

**Important:** Sentry tokens are **organization-scoped**. The user selects which org to authorize during the OAuth flow. To access multiple orgs, create separate grants for each.

## Recommended Scopes

```
org:read project:read team:read member:read event:read
```

Space-separated. Request the minimum you need.

## Example: Create a Grant

```bash
./scripts/tapauth.sh sentry "org:read project:read event:read" "Error Tracker"
```

## Example: List Projects

```bash
curl -H "Authorization: Bearer <token>" \
  https://sentry.io/api/0/projects/
```

## Example: List Issues for a Project

```bash
curl -H "Authorization: Bearer <token>" \
  "https://sentry.io/api/0/projects/{org_slug}/{project_slug}/issues/"
```

## Example: Get Issue Details

```bash
curl -H "Authorization: Bearer <token>" \
  https://sentry.io/api/0/issues/{issue_id}/
```

## Example: List Organizations

```bash
curl -H "Authorization: Bearer <token>" \
  https://sentry.io/api/0/organizations/
```

## Available Scopes

| Scope | Description |
|-------|-------------|
| `org:read` | View organization details |
| `org:write` | Modify organization settings (includes read) |
| `org:admin` | Full org control (includes write) |
| `project:read` | View project details |
| `project:write` | Modify project settings (includes read) |
| `project:admin` | Full project control (includes write) |
| `project:releases` | Manage releases |
| `team:read` | View team details |
| `team:write` | Modify teams (includes read) |
| `team:admin` | Full team control (includes write) |
| `member:read` | View organization members |
| `member:write` | Invite/modify members (includes read) |
| `member:admin` | Remove members (includes write) |
| `event:read` | View events and issues |
| `event:write` | Modify events — resolve, merge, etc. (includes read) |
| `event:admin` | Delete events (includes write) |

## Gotchas

- **Org-scoped tokens:** Each token is tied to the organization the user selected during authorization. You'll only see data from that one org.
- **No revoke endpoint:** Tokens are invalidated by uninstalling the integration from Sentry org settings — there's no programmatic revoke.
- **Scope hierarchy:** `admin` includes `write` includes `read`. Don't request both `event:read` and `event:write` — just use `event:write`.
- **Token lifetime:** Access tokens expire after ~30 days. TapAuth handles refresh automatically.
- **Rate limits:** Sentry enforces rate limits. Check `X-Sentry-Rate-Limit-*` response headers.

## Common Use Cases

| Use Case | API Endpoint |
|----------|-------------|
| List organizations | `GET /api/0/organizations/` |
| List projects | `GET /api/0/projects/` |
| List issues | `GET /api/0/projects/{org}/{project}/issues/` |
| Get issue details | `GET /api/0/issues/{id}/` |
| List events | `GET /api/0/issues/{id}/events/` |
| Resolve an issue | `PUT /api/0/issues/{id}/` (needs `event:write`) |
| List releases | `GET /api/0/organizations/{org}/releases/` |
