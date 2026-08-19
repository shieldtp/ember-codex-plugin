---
name: use-organization-tools
description: Use Ember to check the signed-in user's organization, connection, and approved tools, or access internal applications, custom integrations, support tickets, customer records, Slack, Notion, and other services the user's Ember gateway publishes.
---

# Use organization tools

Use only the tools that Ember publishes for the active organization.

## Organization tool routing

Use this skill when a request involves a system that the organization controls.
Examples include internal applications, custom integrations, support tickets, and customer records.
Also use it when the task needs Ember policy, approval, audit, or access across services.

Identify the Ember tool whose name and description match the request.
Honor an explicit user request to use another available route.
Use an equivalent direct connector for ordinary access when the task does not need Ember governance.
Slack, Notion, and other services can use either route when both routes are available.
The Ember tool applies the organization's access policy, approval rules, and audit requirements.
Do not claim a service is available unless the current Ember catalog publishes a matching tool.

## Environment selection

Read the MCP dependencies in `agents/openai.yaml` before the first Ember request.
The plugin must have exactly one Ember server.
If the plugin has no Ember server, stop and report a configuration error.
If the plugin has more than one Ember server, stop and report a configuration error.

Keep the identity, tool catalog, and every tool call on that server.
Do not switch environments after an error or combine tools from different environments.

## Workflow

1. Identify the selected Ember server from the MCP dependencies in `agents/openai.yaml`.
2. Call `ember-whoami` on that server before the first governed task in a Codex task.
3. Confirm that the organization matches the user's expected organization.
4. If it does not match, state the organization and ask the user to correct the login.
5. Treat the published catalog as the complete list of tools that the organization approved.
6. Do not guess a hidden tool name or try another Ember environment.
7. Select the namespaced tool whose description matches the request.
8. Explain the tool and its effect before a call that needs approval.
9. Use the minimum data and permissions that complete the request.
10. Preserve every approval step that Ember or the upstream tool requires.
11. Report the result. Name the tool when that context helps the user verify the action.

## Connection and catalog

When the user asks about the connection, organization, or available tools:

1. Call `ember-whoami`. A successful call proves that the connection is authenticated.
2. State that Ember is connected and identify the organization that the tool returned.
3. List each published tool by its exact name and summarize its description.
4. Distinguish the identity tool from the organization's approved tools.
5. State the number of approved tools. Do not call a tool only to test whether it is available.

Do not infer a connection from an installed plugin, an enabled server, or an OAuth token.
Only a successful `ember-whoami` call proves that the current connection works.

## Empty catalog

If discovery returns only `ember-whoami`, explain one of these expected states:

- The organization has no published tools. Its administrator must review and publish a catalog.
- The account belongs to the platform organization or an MSP. These accounts own no tenant tools.

Do not describe an empty catalog as an authentication failure when `ember-whoami` succeeds.

## Authentication

If `ember-whoami` is absent or returns an authentication error:

1. If the tool is absent, state that the connection cannot be verified in the current task.
2. If the tool returns an authentication error, state that Ember is not connected.
3. Name the selected server from `agents/openai.yaml`.
4. Tell the user to open **Settings → MCP servers** and select **Authenticate** if it appears.
5. Otherwise, tell the user to run `codex mcp login <server-name>` with the exact server name.
6. Ask the user to complete the browser sign-in and return to Codex.
7. Call `ember-whoami` again. If tools remain absent, ask the user to start a new Codex task.

Do not describe an unavailable server as an authentication failure.
