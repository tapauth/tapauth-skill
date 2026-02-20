# Google Workspace via TapAuth

## Available Scopes

TapAuth supports short scope IDs for readability. Full Google URLs also work.

| Short ID | Access | Full URL |
|----------|--------|----------|
| `drive_read` | Read Google Drive files | `https://www.googleapis.com/auth/drive.readonly` |
| `drive_write` | Full Drive access (read/write) | `https://www.googleapis.com/auth/drive` |
| `calendar_read` | Read calendar events | `https://www.googleapis.com/auth/calendar.readonly` |
| `calendar_events` | Full calendar access | `https://www.googleapis.com/auth/calendar` |
| `sheets_read` | Read Google Sheets | `https://www.googleapis.com/auth/spreadsheets.readonly` |
| `sheets_write` | Full Sheets access | `https://www.googleapis.com/auth/spreadsheets` |
| `docs_read` | Read Google Docs | `https://www.googleapis.com/auth/documents.readonly` |
| `docs_write` | Full Docs access | `https://www.googleapis.com/auth/documents` |
| `contacts_read` | Read contacts | `https://www.googleapis.com/auth/contacts.readonly` |

The following scopes don't have short IDs yet — use the full URL:

| Full URL | Access |
|----------|--------|
| `https://www.googleapis.com/auth/userinfo.email` | Read user email |
| `https://www.googleapis.com/auth/userinfo.profile` | Read user profile |

## Example: List Drive Files

```bash
# 1. Get a token
./scripts/tapauth.sh google "drive_read" "Drive Reader"

# 2. List files
curl -H "Authorization: Bearer <token>" \
  "https://www.googleapis.com/drive/v3/files?pageSize=10"
```

## Example: Read a Google Sheet

```bash
curl -H "Authorization: Bearer <token>" \
  "https://sheets.googleapis.com/v4/spreadsheets/SPREADSHEET_ID/values/Sheet1"
```

## Example: List Calendar Events

```bash
curl -H "Authorization: Bearer <token>" \
  "https://www.googleapis.com/calendar/v3/calendars/primary/events?maxResults=10&timeMin=$(date -u +%Y-%m-%dT%H:%M:%SZ)"
```

## Gotchas

- **Short IDs:** Use short IDs like `drive_read` instead of full URLs. Both formats work.
- **Token refresh:** Google access tokens expire after ~1 hour. TapAuth handles refresh automatically — just call the token endpoint again to get a fresh token.
- **Unverified app warning:** Users may see a "This app isn't verified" screen. They can click "Advanced" → "Go to TapAuth" to proceed.
- **Readonly preference:** Always prefer read-only scope variants unless you need write access. Higher approval rate.
- **Multiple scopes:** Pass as comma-separated or array: `["drive_read", "calendar_read"]`

## Recommended Minimum Scopes

| Use Case | Scopes |
|----------|--------|
| Browse Drive | `drive_read` |
| Read calendar | `calendar_read` |
| Read spreadsheet | `sheets_read` |
| Full workspace | `drive_write`, `calendar_events`, `sheets_write`, `docs_write` |
