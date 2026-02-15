# Google Workspace via TapAuth

## Available Scopes

| Scope | Access |
|-------|--------|
| `https://www.googleapis.com/auth/drive.readonly` | Read Google Drive files |
| `https://www.googleapis.com/auth/drive` | Full Drive access (read/write) |
| `https://www.googleapis.com/auth/calendar.readonly` | Read calendar events |
| `https://www.googleapis.com/auth/calendar` | Full calendar access |
| `https://www.googleapis.com/auth/spreadsheets.readonly` | Read Google Sheets |
| `https://www.googleapis.com/auth/spreadsheets` | Full Sheets access |
| `https://www.googleapis.com/auth/documents.readonly` | Read Google Docs |
| `https://www.googleapis.com/auth/documents` | Full Docs access |
| `https://www.googleapis.com/auth/contacts.readonly` | Read contacts |
| `https://www.googleapis.com/auth/userinfo.email` | Read user email |
| `https://www.googleapis.com/auth/userinfo.profile` | Read user profile |

## Example: List Drive Files

```bash
# 1. Get a token
./scripts/tapauth.sh google "https://www.googleapis.com/auth/drive.readonly" "Drive Reader"

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

- **Scope URLs:** Google scopes are full URLs (not short names like GitHub). Pass them exactly.
- **Token refresh:** Google access tokens expire after ~1 hour. TapAuth handles refresh automatically — just call the token endpoint again to get a fresh token.
- **Unverified app warning:** Users may see a "This app isn't verified" screen. They can click "Advanced" → "Go to TapAuth" to proceed.
- **Readonly preference:** Always prefer `.readonly` scope variants unless you need write access. Higher approval rate.
- **Multiple scopes:** Pass as comma-separated or array: `["https://...drive.readonly", "https://...calendar.readonly"]`

## Recommended Minimum Scopes

| Use Case | Scopes |
|----------|--------|
| Browse Drive | `drive.readonly` |
| Read calendar | `calendar.readonly` |
| Read spreadsheet | `spreadsheets.readonly` |
| Full workspace | `drive`, `calendar`, `spreadsheets`, `documents` |
