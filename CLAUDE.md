# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is the official Claude Code plugin marketplace — a curated registry of plugins (Anthropic-maintained and third-party) that extend Claude Code's capabilities. It is primarily a documentation and configuration repository, not a compiled application.

- `plugins/` — Internal plugins developed and maintained by Anthropic
- `external_plugins/` — Partner/community plugins

## Validation

The only runnable script is the frontmatter validator, which requires [Bun](https://bun.sh):

```bash
# Install dependency (once)
cd .github/scripts && bun install yaml

# Validate all frontmatter files from repo root
bun .github/scripts/validate-frontmatter.ts

# Validate specific files
bun .github/scripts/validate-frontmatter.ts plugins/my-plugin/agents/my-agent.md

# Validate a specific directory
bun .github/scripts/validate-frontmatter.ts plugins/my-plugin/
```

CI runs this automatically on PRs that touch `agents/*.md`, `skills/*/SKILL.md`, or `commands/*.md`.

## Plugin Structure

Every plugin must follow this layout:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Required: name, description, version, author
├── .mcp.json            # Optional: MCP server definitions
├── commands/            # Slash command .md files
├── agents/              # Agent definition .md files
├── skills/              # skill-name/SKILL.md files
└── README.md
```

See `plugins/example-plugin/` for the canonical reference implementation.

## Frontmatter Requirements

All `agents/*.md`, `skills/*/SKILL.md`, and `commands/*.md` files must have valid YAML frontmatter (between `---` delimiters).

**Agents** (`agents/*.md`) — required fields:
- `name` (string)
- `description` (string)

**Skills** (`skills/*/SKILL.md`) — required fields:
- `description` OR `when_to_use` (string)

**Commands** (`commands/*.md`) — required fields:
- `description` (string)

Glob patterns and other values containing YAML special characters (`{}[]*&#!|>%@``) do not need manual quoting — the validator handles them automatically.

## Marketplace Registry

`/.claude-plugin/marketplace.json` is the central registry listing every available plugin with its metadata, source path, category, and (for LSP plugins) server configuration. Update this file when adding or removing plugins.

## Contribution Policy

Only Anthropic team members can merge changes. The `close-external-prs.yml` workflow automatically closes PRs from non-members. External plugin submissions go through the [plugin directory submission form](https://clau.de/plugin-directory-submission).
