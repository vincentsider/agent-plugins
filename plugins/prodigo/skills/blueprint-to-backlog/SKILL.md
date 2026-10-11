---
name: blueprint-to-backlog
description: Use when the user has a Pega Blueprint (JSON export or pasted content) and wants a Jira backlog, a completeness check of the blueprint, or the open questions to answer before building stories. Not for stories without a blueprint (use story-quality).
---

# Blueprint to backlog

## Inputs

The Blueprint JSON (a Pega Blueprint export has `pyCandidateCaseTypes` at the root or under
`pyDADInfo`). Ask for the file or the JSON if it is not in the conversation. Ask which Jira
project key to use if the account has more than one.

## Procedure

1. `ingest_blueprint(blueprint, project_key)`: parses the blueprint into a preview tree of
   epics, stories and sub-tasks. No Jira write. Show the tree and the counts.
2. `analyze_blueprint(blueprint)`: completeness score, strengths and gaps per case type,
   prioritised recommendations. Show the score and the top three gaps; a weak blueprint
   produces weak stories, so recommend fixing blocking gaps in Pega first.
3. `get_blueprint_questions(blueprint)`: the clarifying questions whose answers make stories
   unambiguous (roles, approvers, decision criteria, exceptions, SLAs). Ask the user; keep
   their answers in the conversation so the backlog step can use them.
4. Confirm the write: "Create N epics, M stories and K sub-tasks in project X?". Only after a
   yes call `create_backlog_from_blueprint(blueprint, project_key, dry_run=false)`.
   Use `dry_run=true` first when the user wants to see counts without writing.
5. Report the created issue keys and the browse URLs exactly as returned.

## After the backlog exists

- Offer `story-quality` to optimise the created stories against the Definition of Ready, and
  `judge` once the application exists.
- If a Pega application is connected, mention that grounding was applied; otherwise say the
  backlog is ungrounded.

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

