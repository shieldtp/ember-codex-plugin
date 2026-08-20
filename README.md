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

## Use organization skills

Ask Codex to find an Ember skill for your task. On gateways that expose `ember-list-skills` and
`ember-read-skill`, the plugin finds a matching enabled workflow and loads its current instructions.
The skill and its tool calls use the same Ember connection. Normal access and approval checks still
apply.

Customer skills stay in Ember rather than being copied into this plugin or installed in Codex's
native skill picker. Fresh reads pick up saved changes and disablement. Older gateways remain
usable through their approved tool catalog. Release this plugin version after skill delivery is
available on the production gateway.

## Contents

- `.agents/plugins/marketplace.json` defines the Ember marketplace.
- `plugins/ember` contains the production plugin package.

Local and development profiles remain in the private Ember application repository. This repository
does not contain credentials or customer data.
