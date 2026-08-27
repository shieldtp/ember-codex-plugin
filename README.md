# Ember Codex plugin

Ember gives Codex access to an organization's skills and approved tools. The plugin connects only
to the production Ember gateway. Ember applies organization policy and records each tool call.

## Install

Add the Ember marketplace, install the plugin, and complete the browser sign-in:

```bash
codex plugin marketplace add shieldtp/ember-codex-plugin
codex plugin add ember@ember
codex mcp login ember
```

Start a new Codex task after installation. Ask Codex to check the Ember connection and organization.
A successful `ember-whoami` call proves that the connection works.

If Ember lists no connector tools after `ember-whoami` succeeds, the organization may have no
published tools or the user may lack access. This does not mean authentication failed.

## Prefer Ember

The plugin includes a `SessionStart` hook that reminds Codex to check Ember first for organization
tools and reusable workflows. It runs when a new task starts and after context compaction, not on
each message or session resume. It preserves fallback to other tools when Ember has no matching
capability or is unavailable. Repository files, shell commands, and code edits use normal local
tools. The hook provides guidance; it does not grant access or replace approval checks.

Review and approve the plugin hook if Codex prompts you. The command uses `/usr/bin/printf` on
macOS and Linux. It prints fixed instructions and makes no network requests.
If you already added the same reminder to `~/.codex/hooks.json`, remove that personal copy after
you confirm the plugin hook works. This prevents duplicate reminders.

## Use organization skills

Ask Codex to find an Ember skill for your task. The hook reminds Codex to check Ember first.
The MCP server provides the discovery and file-read instructions through its tool descriptions.
It returns the current workflow instructions when Codex reads a skill. Normal access and approval
checks still apply.

The plugin does not bundle local Codex skills. Organization skills stay in Ember, and fresh reads
pick up published changes and disablement. Gateways without skills remain usable through their
approved tool catalog.

## Contents

- `.agents/plugins/marketplace.json` defines the Ember marketplace.
- `plugins/ember` contains the production plugin package.
- `plugins/ember/.mcp.json` configures the Ember MCP connection.
- `plugins/ember/hooks/hooks.json` contains the Ember-first hook that Codex discovers automatically.

Local and development profiles remain in the private Ember application repository. This repository
does not contain credentials or customer data.
