---
name: mcp-readiness
description: Use when the user asks whether a Pega application is safe to expose as an MCP server, wants an audit of which case types, flows, reports or data pages to expose, wants to register a Pega Service MCP endpoint, or wants MCP tool descriptions measured or rewritten for reliable agent selection.
---

# MCP readiness

## Audit before exposing

`audit_mcp_readiness(app_name, audience, language)` classifies every exposable artifact of the
connected application as RECOMMENDED_EXPOSE, EXPOSE_WITH_GUARDRAIL, DO_NOT_EXPOSE or
INSUFFICIENT_INFORMATION, each with a reason grounded in the rules. Needs a connected
application (see `retrodoc`). Present the DO_NOT_EXPOSE list first.

## Register a Service MCP

`connect_service_mcp(endpoint_url)` registers the Infinity 26 Service MCP endpoint once for
this account; later story and design work is grounded in its tools. Ask the user to paste the
endpoint URL from Pega; it is not derivable.

## Measure and improve tool descriptions

- `judge_mcp_tools(tool_manifest, tasks_per_tool)` measures how reliably a separate model
  picks the right tool from the descriptions and diagnoses each weak one.
- `optimize_mcp_tool_descriptions(tool_manifest, tasks_per_tool)` rewrites the names and
  descriptions and proves the before and after selection accuracy. Show the delta per tool.

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

