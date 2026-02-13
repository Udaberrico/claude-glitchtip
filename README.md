# GlitchTip Plugin for Claude Code

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that integrates [GlitchTip](https://glitchtip.com) error monitoring. Based on [mcp-glitchtip](https://github.com/coffebar/mcp-glitchtip).

Fetch and analyze issues, stack traces, and error context directly from your GlitchTip instance within Claude Code.

## Installation

```
/plugin marketplace add Udaberrico/udaberrico-claude-marketplace
/plugin install glitchtip@udaberrico-claude-marketplace
```

Other useful commands:

```
/plugin list
/plugin update glitchtip
/plugin uninstall glitchtip
/plugin marketplace update
```

## Setup

Set the following environment variables in your shell (e.g. `~/.zshrc` or `~/.bashrc`):

```bash
export GLITCHTIP_TOKEN="your-api-token"
export GLITCHTIP_ORGANIZATION="your-org-slug"
export GLITCHTIP_BASE_URL="https://your-glitchtip-instance.com"
```

### Getting your API token

1. Log in to your GlitchTip instance
2. Navigate to **Profile > Auth Tokens** (`/profile/auth-tokens`)
3. Generate a new token

### Environment variables

| Variable | Required | Description |
|---|---|---|
| `GLITCHTIP_TOKEN` | Yes | API token for authentication |
| `GLITCHTIP_ORGANIZATION` | Yes | Your organization slug |
| `GLITCHTIP_BASE_URL` | No | Your GlitchTip instance URL (defaults to `https://app.glitchtip.com`) |

## Available tools

| Tool | Description |
|---|---|
| `glitchtip_list_projects` | List all projects in the organization |
| `glitchtip_list_issues` | List issues, optionally filtered by project and search query (unresolved by default) |
| `glitchtip_get_event` | Get the latest event for a specific issue with full stack trace and error context |
| `glitchtip_resolve_issue` | Mark an issue as resolved |
| `glitchtip_ignore_issue` | Mark an issue as ignored |

## Usage

Use the `/glitchtip` slash command in Claude Code to start a guided error triage workflow. It will:

1. Detect your project automatically (or ask you to pick one)
2. Fetch and summarize all unresolved issues
3. Walk you through investigating each issue with full stack traces
4. Search your codebase for the relevant code and suggest fixes
5. Offer to resolve or ignore issues once addressed

You can also ask Claude directly:

- "Show me all unresolved GlitchTip errors"
- "What's the latest error in GlitchTip?"
- "Get the stack trace for GlitchTip issue 42"
- "Check if there are any production errors"

## License

MIT
