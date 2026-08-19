# Ember Codex plugin

Ember gives Codex access to the tools that an organization approves. The plugin connects only to
the production Ember gateway. Ember applies organization policy and records each tool call.

## Install

Add the Ember marketplace, install the plugin, and complete the browser sign-in:

```bash
codex plugin marketplace add shieldtp/ember-codex-plugin
codex plugin add ember@ember
codex mcp login ember
```

Start a new Codex task after installation. Ask Codex to check the Ember connection and organization.
A successful `ember-whoami` call proves that the connection works.

If Ember lists no organization tools after `ember-whoami` succeeds, an administrator has not
published a tool catalog for that organization.

## Contents

- `.agents/plugins/marketplace.json` defines the Ember marketplace.
- `plugins/ember` contains the production plugin package.

Local and development profiles remain in the private Ember application repository. This repository
does not contain credentials or customer data.
