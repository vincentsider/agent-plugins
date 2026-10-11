# Prodigo for Pega

Grounded Pega delivery from your coding agent: Blueprint to backlog, story quality, Pega
Infinity technical designs, builds into an isolated Pega branch, documentation of existing
applications, an independent judge, and migration planning. Everything runs on the Prodigo
service; this package contains skills and manifests only. Nothing to install locally, no
Java, no credentials in chat.

Works with Pega Infinity 24.2 and 26. Prodigo is not affiliated with or endorsed by
Pegasystems. Pegasystems, Pega, and the Pega logo are trademarks or registered trademarks of
Pegasystems Inc. and/or its affiliates. All other trademarks are the property of their
respective owners.

## Install

| Host | Commands |
|---|---|
| Claude Code | `claude plugin marketplace add vincentsider/agent-plugins` then `claude plugin install prodigo@prodigo` |
| Codex CLI / ChatGPT desktop | `codex plugin marketplace add vincentsider/agent-plugins` then `codex plugin add prodigo@prodigo` (or `/plugins`) |
| GitHub Copilot CLI | `copilot plugin marketplace add vincentsider/agent-plugins` then `copilot plugin install prodigo@prodigo` |
| Cursor | copy `plugins/prodigo` into your plugins folder (root `plugin.json` is Agent Plugins 1.0) |
| OpenClaw | `openclaw plugins install prodigo --marketplace github.com/vincentsider/agent-plugins` |
| claude.ai, Claude Desktop, mobile, Cowork | add the custom connector `https://app.prodigo.ai/mcp` (or pick "Prodigo for Pega" in the Connectors Directory once listed) |

After installing, start a new session and ask the assistant to run `prodigo-setup`. The
first tool call opens a browser window to sign in to your Prodigo workspace (OAuth 2.1);
Jira, Confluence and Pega are connected on the Prodigo web app, never typed into chat.

## What you need on the Pega side

A development or UAT Pega environment reachable from Prodigo (its outbound IP allow-listed
on your Pega host), a developer operator login, the application name, version and ruleset,
and, for builds, an OAuth client for the Build MCP. See the setup skill for the full list.

## Skills

`prodigo-setup`, `prodigo-assistant`, `blueprint-to-backlog`, `story-quality`, `house-style`,
`technical-design`, `build-to-branch`, `retrodoc`, `judge`, `migration-planner`,
`mcp-readiness`.

## Privacy and terms

https://prodigo.ai/privacy and https://prodigo.ai/terms. Support: plugins@prodigo.ai.

## Development

`claude plugin validate ./plugins/prodigo --strict` and `claude plugin eval ./plugins/prodigo`
(suite under `evals/`). The MCP server behind this plugin is `https://app.prodigo.ai/mcp`
(Streamable HTTP, OAuth 2.1 with dynamic client registration and client ID metadata
documents).
