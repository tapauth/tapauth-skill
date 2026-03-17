# Contributing to TapAuth Agent Skill

## How It Works

This skill is maintained by the TapAuth team. The source of truth lives in our internal systems and is synced to this public repo automatically.

## Reporting Issues

If you find a bug, incorrect documentation, or a missing provider:

1. [Open an issue](https://github.com/tapauth/skill/issues) on this repo
2. Include the provider name, what you expected, and what happened
3. We'll triage and fix it in the next sync

## Pull Requests

We welcome PRs for:

- Typo fixes and documentation improvements
- New provider reference docs (see `references/` for the format)
- Bug fixes in `scripts/tapauth.sh`

Please keep PRs focused on a single change.

## Publishing to ClawHub

Only maintainers publish to ClawHub. After changes are merged:

```bash
clawhub publish /path/to/skill/ \
  --slug tapauth \
  --name "TapAuth" \
  --version <new-version> \
  --changelog "Description of changes"
```

## Git Identity

Automated commits use:

```
TapAuth[bot] <2825152+TapAuth[bot]@users.noreply.github.com>
```
