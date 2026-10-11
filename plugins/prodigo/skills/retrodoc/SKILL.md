---
name: retrodoc
description: Use when the user wants documentation of an existing Pega application, a reverse-engineered functional or technical description, a backlog derived from what already exists, or asks to "connect" a Pega application for grounding. Not for verifying documentation (judge).
---

# Retrodoc: document an existing application

## Connect the application (once per session)

`connect_pega_project(application, version, rulesets)` fetches the rules, builds the
dependency graph, binds the application to this session and indexes it. It is metered:
confirm with the user before calling. On success later tools ground themselves in it. If the
fetch fails with a network error, see the troubleshooting note in `prodigo-setup`.

## Document

Two sources, pick by what exists:

| Source | Tool |
|---|---|
| Live application, by crawling its screens (needs the portal URL and a role) | `start_retrodoc(pega_app_url, role_label, mode, language, audience, max_screens)` then poll `get_retrodoc_status(task_id)` then `get_retrodoc_document(task_id)` |
| Rule export plus a screen crawl combined (the default when both exist) | `document_pega_from_rules(app_name, app_label, audience, language, pega_app_url, combine_screens)` |

`mode` for `start_retrodoc`: `auto` uses the connected rules plus the screens, `screens`
uses the live UI only. `language` follows the user's language. `audience` is `business` or
`technical`.

Crawls take minutes; poll every 30 seconds and relay the `k/N screens` progress.

## Derive a backlog

`create_backlog_from_retrodoc(task_id, project_key, dry_run=true)` previews the epics,
stories and sub-tasks derived from the document; after the user's yes call it with
`dry_run=false`. Report the created keys.

## Then prove it

Offer `judge_document` (see `judge`) so an independent model checks the document against the
running application before anyone relies on it.

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

