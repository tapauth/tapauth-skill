# Gmail via TapAuth

## Available Scopes

TapAuth supports short scope IDs for readability. Full Google URLs also work.

| Short ID | Access | Full URL |
|----------|--------|----------|
| `gmail_read` | Read emails and labels | `https://www.googleapis.com/auth/gmail.readonly` |
| `gmail_send` | Send emails only | `https://www.googleapis.com/auth/gmail.send` |

The following Gmail scopes don't have short IDs yet — use the full URL:

| Full URL | Access |
|----------|--------|
| `https://www.googleapis.com/auth/gmail.modify` | Read, send, and modify (labels, archive) |
| `https://www.googleapis.com/auth/gmail.compose` | Create and send drafts |
| `https://www.googleapis.com/auth/gmail.labels` | Manage labels |
| `https://mail.google.com/` | Full Gmail access (avoid unless necessary) |

## Example: Read Recent Emails

```bash
# 1. Get a token
./scripts/tapauth.sh gmail "gmail_read" "Email Reader"

# 2. List messages
curl -H "Authorization: Bearer <token>" \
  "https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=5"

# 3. Read a specific message
curl -H "Authorization: Bearer <token>" \
  "https://gmail.googleapis.com/gmail/v1/users/me/messages/MSG_ID?format=full"
```

## Example: Send an Email

```bash
# Base64url-encode the email
EMAIL=$(printf "To: recipient@example.com\r\nSubject: Hello\r\nContent-Type: text/plain\r\n\r\nHello from my agent!" | base64 | tr '+/' '-_' | tr -d '=')

curl -X POST -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  "https://gmail.googleapis.com/gmail/v1/users/me/messages/send" \
  -d "{\"raw\": \"$EMAIL\"}"
```

## Gotchas

- **Provider name:** Use `gmail` (not `google`) when creating the grant — TapAuth treats Gmail separately for scope clarity.
- **Short IDs:** Use `gmail_read` and `gmail_send` instead of full URLs. Both formats work.
- **Token refresh:** Same as Google — tokens expire after ~1 hour. Re-call the token endpoint for a fresh one.
- **Sensitive scopes:** Gmail scopes are classified as "sensitive" by Google. Users will see an extra confirmation screen.
- **Email format:** The Gmail API expects RFC 2822 formatted emails, base64url-encoded. Use the raw format.
- **Rate limits:** 250 quota units/second per user. Most operations cost 5-10 units.

## Recommended Minimum Scopes

| Use Case | Scopes |
|----------|--------|
| Read inbox | `gmail_read` |
| Send emails | `gmail_send` |
| Read + send | `gmail_read`, `gmail_send` |
| Full management | `https://www.googleapis.com/auth/gmail.modify` (no short ID) |
