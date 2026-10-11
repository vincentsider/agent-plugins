---
name: judge
description: Use when the user asks to verify, prove, check or audit a document, a user story or a backlog against the real running Pega application, or asks how trustworthy generated output is. Runs Prodigo's independent judge. Not for writing stories or documents.
---

# Judge: independent verification

The judge is a separate model with a sceptical prompt that inspects the running application
itself (screens and rules) and rules each claim CONFIRMED, CONTRADICTED, GAP or UNVERIFIABLE.
It never grades its own work. Expect minutes, not seconds.

| Target | Tool |
|---|---|
| One Jira story's acceptance criteria | `verify_story(story_key, project_key, pega_app_url)` (synchronous, posts a report to Jira) |
| The whole backlog | `start_verify_backlog(project_key, pega_app_url, limit)` then poll `get_verify_backlog_status(task_id)` |
| A generated document | `judge_document(app_name, pega_app_url, role_label)` |
| Written backlog stories against the live app | `judge_backlog(project_key, app_name, pega_app_url, role_label, limit, language)` |
| Persist a verification you performed with other evidence | `verification_report(story_key, overall, criteria, title, evidence)` |

## Reading verdicts

- CONFIRMED and PASS: evidence observed on screen; quote the evidence line.
- GAP: committed but not implemented; the remediation field says what to build.
- CONTRADICTED and FAIL: the application behaves differently; this is the finding to escalate.
- UNVERIFIABLE: the judge could not reach the screen; say so, do not guess.

Never soften a verdict. Report counts per verdict and list every CONTRADICTED item with its
evidence. Offer `build-to-branch` or `story-quality` to close GAPs.

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

