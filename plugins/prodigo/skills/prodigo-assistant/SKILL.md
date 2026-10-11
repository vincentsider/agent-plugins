---
name: prodigo-assistant
description: Use whenever the task involves Pega delivery through Prodigo (backlogs, user stories, technical designs, builds into a Pega branch, documenting an existing Pega application, verifying it, migrating it) and no more specific Prodigo skill matches. Explains which workflow to pick and the rules every Prodigo call follows.
---

# Prodigo assistant

Pick the workflow, then load the matching skill:

| The user wants to | Skill |
|---|---|
| Turn a Pega Blueprint into a Jira backlog | `blueprint-to-backlog` |
| Write, improve or create user stories that meet the Definition of Ready | `story-quality` |
| Make stories follow the client's own format and method | `house-style` |
| Produce a Pega Infinity technical design from a story | `technical-design` |
| Build a design into a Pega branch, or finish deferred rules | `build-to-branch` |
| Document an existing Pega application and derive a backlog | `retrodoc` |
| Prove a document or backlog against the live application | `judge` |
| Plan and execute a migration into Pega | `migration-planner` |
| Audit a Pega app before exposing it as an MCP server, or improve MCP tool descriptions | `mcp-readiness` |
| Install, sign in, connect, troubleshoot | `prodigo-setup` |

Start every new session with `get_profile` then `list_capabilities`; branch on the result.

## Grounding first

When a Pega application is connected (`pega_rules` true), run `connect_pega_project` once
per session for the application the user names (it is metered: confirm first), then let the
story, design and documentation tools ground themselves in its real rules. Ungrounded output
is allowed but say so.

## Working with Pega's own plugin

If the `pega-infinity-authoring` plugin is installed, use it for hands-on edits of single
rules through a ChangeRequest. Use Prodigo to generate designs, build whole designs into a
branch, document and verify. Never mix the two in one branch without telling the user.

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

