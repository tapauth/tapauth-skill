# TapAuth Agent Skill

> OAuth token broker for AI agents. One API call to connect any OAuth provider.

This is the official [Agent Skill](https://agentskills.io) for [TapAuth](https://tapauth.com) — the trust layer between humans and AI agents.

## Install

Works with any agent that supports the [Agent Skills standard](https://agentskills.io):

```bash
npx skills add tapauth/tapauth-skill
```

Compatible with: **Claude Code** · **Cursor** · **OpenClaw** · **OpenAI Codex** · **GitHub Copilot** · **VS Code** · and more.

## What It Does

Gives your AI agent the ability to get OAuth tokens from users. Instead of hardcoding API keys or passing credentials, TapAuth lets users approve access in their browser with granular scope control.

```
Agent creates grant → User approves in browser → Agent gets scoped token
```

No API key needed. No signup needed. The user's approval is the only gate.

## Supported Providers

| Provider | Reference | Scopes |
|----------|-----------|--------|
| GitHub | [references/github.md](references/github.md) | `repo`, `read:user`, `workflow`, etc. |
| Google Workspace | [references/google.md](references/google.md) | Drive, Calendar, Sheets, Docs |
| Gmail | [references/gmail.md](references/gmail.md) | Read, send, manage emails |
| Linear | [references/linear.md](references/linear.md) | Issues, projects, teams |

More providers coming soon: Slack, Discord, Notion, Vercel.

## Quick Example

```bash
# Your agent runs this:
curl -X POST https://tapauth.com/api/grants \
  -H "Content-Type: application/json" \
  -d '{"provider": "github", "scopes": ["repo"], "agent_name": "My Agent"}'

# User clicks the approval URL
# Agent polls for the token
# Agent uses the token to access GitHub on the user's behalf
```

## Links

- 🌐 [tapauth.com](https://tapauth.com)
- 📖 [Documentation](https://tapauth.com/docs)
- 🔐 [Agent Skills Spec](https://agentskills.io)

## License

MIT
