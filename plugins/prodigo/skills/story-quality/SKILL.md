---
name: story-quality
description: Use when the user wants a user story written, optimised, created in Jira, checked against a Definition of Ready, or an epic's stories improved in bulk; also when they say "implement", "refine" or "DoR" about a Jira key. Not for blueprints (blueprint-to-backlog) or verification (judge).
---

# Story quality

## Choose the tool

| Situation | Tool |
|---|---|
| A story text, improve it, no Jira write | `optimize_user_story(story, definition_of_ready, sme_instructions, ...)` |
| Write a new story into Jira | `start_create_and_optimize_user_story(...)` then poll `get_user_story_status(task_id)` |
| Improve every story under an epic or project | `start_optimize_jira_epic_subtasks(project_key, ...)` then poll `get_jira_batch_status(task_id)` |
| The user names a key such as PPT-5589 | `get_jira_story(story_key)` first; never ask them to paste it |
| The client has examples of good stories | `get_reference_stories(project_key)` and pass the best one as `example_user_story` |

Prefer the `start_*` variants: a swarm run takes one to three minutes and some clients cap
tool calls at 60 to 120 seconds.

## Definition of Ready and SME instructions

Pass the user's Definition of Ready verbatim as `definition_of_ready`; pass domain rules as
`sme_instructions`. If a house style has been learned (see `house-style`), it applies
automatically and the result includes a conformance panel. Do not restate rules the tool
already enforces.

## Grounding

When a Pega application is connected, the story is grounded in its real rules and the result
carries a grounding evidence section. If the result says "not grounded", offer
`connect_pega_project` (metered, confirm first).

## Output

Show the optimised story as returned, its priority score and reasoning, and the Jira key and
URL when one was created. For batch runs, report the final X/Y count and any failures by key.

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

