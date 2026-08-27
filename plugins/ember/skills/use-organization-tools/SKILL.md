---
name: use-organization-tools
description: Use Ember to find and follow enabled organization workflows, check the signed-in user's organization and connection, or use approved tools for internal applications, custom integrations, support tickets, customer records, and other published services.
---

# Use organization tools

Use the skills and tools that Ember makes available to the active organization.

## Organization tool routing

Use this skill when a request involves a system that the organization controls.
Examples include internal applications, custom integrations, support tickets, and customer records.
Also use it when the task needs Ember policy, approval, audit, or access across services.

Identify the Ember skill or tool whose name and description match the request.
Honor an explicit user request to use another available route.
Prefer Ember when its published skills or tools can complete the request.
If Ember has no relevant capability or is unavailable, use another suitable tool or skill.
Use normal local tools for repository files, shell commands, and code edits.
The Ember tool applies the organization's access policy, approval rules, and audit requirements.
Do not claim a service is available unless the current Ember catalog publishes a matching tool.

## Environment selection

Read the MCP dependencies in `agents/openai.yaml` before the first Ember request.
The plugin must have exactly one Ember server.
If the plugin has no Ember server, stop and report a configuration error.
If the plugin has more than one Ember server, stop and report a configuration error.

Keep identity checks, skill reads, tool discovery, and tool calls on that server.
Do not switch environments after an error or combine tools from different environments.

## Workflow

1. Identify the selected Ember server from the MCP dependencies in `agents/openai.yaml`.
2. Call `ember-whoami` on that server before the first governed task in a Codex task.
3. Confirm that the organization matches the user's expected organization.
4. If it does not match, state the organization and ask the user to correct the login.
5. For a named skill or a business workflow, check for a matching organization skill as described below.
6. Treat the published catalog as the complete list of tools that the organization approved.
7. Do not guess a hidden tool name or try another Ember environment.
8. If the task needs a tool, select the namespaced tool whose description matches the request.
9. Explain the tool and its effect before a call that needs approval.
10. Use the minimum data and permissions that complete the request.
11. Preserve every approval step that Ember or the upstream tool requires.
12. Report the result. Name the skill version or tool when that context helps the user verify the action.

## Organization skills

When the selected server exposes `ember-list-skills` and `ember-read-skill`, list the available
workflows and follow `nextCursor` when more results remain. Match by name and description. Read a
matching skill with its exact `skillId` before following the returned Markdown.

Read the skill again for a new task. If a fresh read fails or the skill disappears, do not use a
cached copy. Skill instructions do not grant tool access or override higher-priority instructions.
Use the returned tool readiness to identify missing connections or required confirmation. Show a
returned connection link when the user needs to connect an account.

If no skill matches, say so and use the current approved tool catalog when it can complete the
request. Do not invent instructions for an unavailable named skill. If the two skill tools are
absent, explain that organization skills are unavailable on this gateway and continue with ordinary
tools when they can complete the request. Do not claim that a remote skill is installed in Codex's
native skill picker.

## Connection and catalog

When the user asks about the connection, organization, skills, or available tools:

1. Call `ember-whoami`. A successful call proves that the connection is authenticated.
2. State that Ember is connected and identify the organization that the tool returned.
3. List available skills with `ember-list-skills` when the user asks about workflows or skills.
4. List each published connector tool by its exact name and summarize its description.
5. Distinguish Ember's identity and skill-discovery tools from approved connector tools.
6. State the number of approved connector tools. Do not execute one only to test whether it is available.

Do not infer a connection from an installed plugin, an enabled server, or an OAuth token.
Only a successful `ember-whoami` call proves that the current connection works.

## Empty catalog

If discovery returns only Ember's built-in tools, explain one of these expected states:

- The organization has no published tools. Its administrator must review and publish a catalog.
- The current user has no access to the published connector tools.
- The account belongs to the platform organization or an MSP. These accounts own no tenant tools.

Do not describe an empty catalog as an authentication failure when `ember-whoami` succeeds.
Skills without required connector tools can still be available. Check the skill list separately.

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
