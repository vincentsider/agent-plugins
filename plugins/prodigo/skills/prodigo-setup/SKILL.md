---
name: prodigo-setup
description: Use when the user installs Prodigo for the first time, asks to connect Prodigo, cannot sign in, or a Prodigo tool answers "not signed in" or that a connection (Jira, Confluence, Pega) is missing. Checks the account, lists connections and routes the user to the right configuration page. Not for doing delivery work itself.
---

# Prodigo setup

Prodigo for Pega is a remote service. The plugin only ships instructions; everything runs on
the user's Prodigo workspace over an authenticated connection.

## Step 1: confirm the connection

1. Call `get_profile`. If it returns an `id`, the user is signed in; continue.
2. If the tool is unavailable or the client reports an authentication error, the OAuth
   sign-in has not completed. Tell the user to re-run the connect step of their client
   (for example `/mcp` in Claude Code, "Connect" on the plugin page in ChatGPT or Codex)
   and sign in with their Prodigo account in the browser window that opens. Do not ask for
   credentials in chat.

## Step 2: list what this account can do

Call `list_capabilities`. It returns booleans for `jira`, `confluence`, `pega_rules`,
`pega_service_mcp` and `build_mcp`, plus a `configure_at` URL (the Prodigo web app).
"Prodigo Settings" below means: open `configure_at`, start or open a conversation, and
use the settings gear in the conversation header.

| Missing | What the user needs | Where |
|---|---|---|
| `jira` | Jira host, e-mail and API token of their own account | Prodigo Settings, Jira |
| `confluence` | Same account; Confluence lives at the Jira host's `/wiki` | Prodigo Settings, Confluence |
| `pega_rules` | A developer operator login on a Pega dev or UAT environment (never production), the application name, version and ruleset | Prodigo Settings, Pega application |
| `pega_service_mcp` | The Infinity 26 Service MCP endpoint URL (pasted once with `connect_service_mcp`) | chat or Settings |
| `build_mcp` | Operator-side: the Build MCP sidecar with a service-account OAuth client | Prodigo operator |

Explain only the missing ones, in one sentence each, and stop. Do not try to work around a
missing connection.

## Step 3: a first task

Suggest one of these, matching what is connected:

- Jira only: "optimize this user story" (see the story-quality skill).
- Jira plus Pega: "connect the Pega application and ground the next stories" (see retrodoc
  and story-quality).
- Pega and Build MCP: "write a technical design for PPT-123 and build it into a branch".

## Troubleshooting

- "This workspace has used today's included tool calls": the daily quota of this plan is
  reached; it resets at 00:00 UTC. Point the user to the informational page named in the
  message. Do not describe prices.
- A Pega fetch fails with a network error: the Pega host must allow Prodigo's outbound IP.
  The Prodigo operator publishes the address; the user's Pega administrator allow-lists it.
- Tools keep answering "not signed in" after a successful browser sign-in: the client cached
  an old session. Start a new session in the client.

## Operating rules (apply to every Prodigo workflow)

- Connections to Jira, Confluence and Pega are configured on the Prodigo web app. Never ask
  the user for a password, API token or client secret in chat. If `list_capabilities` says a
  connection is missing, send the user to the `configure_at` URL it returns and stop.
- Long jobs are asynchronous. A `start_*` tool returns a job handle; poll the matching
  `get_*_status` tool every 15 to 30 seconds, then fetch the report. Never start the same
  job twice.
- Writes need consent. Before creating Jira issues, publishing to Confluence or building into
  a Pega branch, state exactly what will be written and wait for the user's yes.
- Prodigo builds into an isolated Pega branch and never merges. A human reviews and merges.
- Report what the tools returned. Do not invent issue keys, rule names or verdicts.

