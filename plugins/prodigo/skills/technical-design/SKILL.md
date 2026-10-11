---
name: technical-design
description: Use when the user asks for a Pega technical design, "how would we build this in Pega", the rules needed for a story, or says "implement" or "design" with a Jira key. Produces a Pega Infinity design grounded in the connected application. Not for building (build-to-branch).
---

# Technical design

## Procedure

1. Identify the story. If the user names a Jira key, pass it as `story_key`; the story is
   read from Jira. Otherwise pass the full text as `story`. Never ask the user to paste a
   story that already exists in Jira.
2. Call `generate_pega_technical_design(story | story_key, project_key)`.
3. Show the design as returned: case type, stages and steps, data model, decision and
   validation rules, integrations, UI, and the rule list. Keep the markdown intact; the build
   step parses it.

## Grounding

With a connected Pega application the design references the real classes, rulesets and
existing rules, and lists what already exists versus what is new. Without one, say the
design is ungrounded and offer `connect_pega_project` (metered, confirm first).

## Next steps to offer

- `build-to-branch` to author the rules into an isolated branch.
- `migration-planner` when the user wants a reviewable plan with per-rule verdicts before any
  build, or when the target application already contains similar rules.

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

