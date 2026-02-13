# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code plugin marketplace containing plugins distributed via `claude plugin marketplace add`. Currently ships one plugin: **GlitchTip** (error monitoring integration).

## Repository Structure

```
.claude-plugin/marketplace.json    # Marketplace manifest — declares available plugins
plugins/<name>/                    # Each plugin lives here
  .claude-plugin/plugin.json       # Plugin metadata (name, description, author)
  .mcp.json                        # MCP server launch config (command, args, env)
  CLAUDE.md                        # Instructions Claude receives when plugin is active
  bundle.mjs                       # Built MCP server (single-file, committed to repo)
  skills/<name>/SKILL.md           # Slash command workflows
  mcp-server/                      # TypeScript source for the MCP server
```

## Build

The MCP server source is in `plugins/glitchtip/mcp-server/`. The built `bundle.mjs` is committed to the repo (it's what gets distributed).

```bash
cd plugins/glitchtip/mcp-server
npm install
npm run build          # tsc && node esbuild.config.js → outputs ../bundle.mjs
npm run dev            # tsc --watch (no esbuild, no bundle)
```

After changing MCP server code, always rebuild and commit the updated `bundle.mjs`.

## Plugin Architecture

Each plugin has three integration layers with Claude Code:

1. **MCP Tools** (`.mcp.json` + `bundle.mjs`) — Low-level capabilities exposed as tools. The server uses `@modelcontextprotocol/sdk` with `StdioServerTransport`. Tool parameters are validated with `zod`.
2. **CLAUDE.md** — System-level instructions telling Claude when/how to use the tools proactively.
3. **Skills** (`skills/*/SKILL.md`) — Step-by-step workflows triggered by slash commands (e.g., `/glitchtip`).

The `.mcp.json` uses `${CLAUDE_PLUGIN_ROOT}` to resolve paths relative to the plugin directory at runtime.

## Adding a New Plugin

1. Create `plugins/<name>/` with `.claude-plugin/plugin.json`, `.mcp.json`, `CLAUDE.md`
2. Add the plugin entry to `.claude-plugin/marketplace.json`
3. Build and include `bundle.mjs` if it has an MCP server

## No CI/CD, Linting, or Tests

There is currently no test suite, linter, or CI pipeline configured.
