---
name: build-to-branch
description: Use when the user wants a technical design built into Pega, asks to "create the rules", "build this into a branch", finish rules that need inputs, run the build-judge-fix loop, or list and update the branches Prodigo built. Writes to a Pega branch through Pega's Build MCP; never merges. Not for designing (technical-design).
---

# Build to branch

## Preconditions

`list_capabilities` must report `build_mcp` true and `pega_rules` true. The design must be
the markdown produced by `generate_pega_technical_design` (pass it unchanged as `design`).

## Procedure

1. Confirm the write: name the application, the origin ruleset and the branch label, and say
   that nothing is merged. Offer `dry_run=true` first to preview the rule list.
2. `start_build(design, branch, application, origin_ruleset, applies_to_class, detail, dry_run)`
   returns a `task_id`.
3. Poll `get_build_status(task_id)` every 20 to 30 seconds until done, then
   `get_build_report(task_id)`.
4. The report lists built rules and DEFERRED rules that need the user's input (for example a
   Connect-REST endpoint). Ask for exactly those inputs, then call `start_build` again with
   the SAME `branch` and `rule_inputs` as a JSON array of
   `{"rule_name", "endpoint_url", "http_method"?, "auth_profile"?, "applies_to"?}`. The
   existing branch and change request are reused; nothing else is rebuilt.
5. Report built, deferred and failed rules by name, and the branch name, exactly as returned.

## Close the loop

`start_remediation_loop(design, branch, app_url, story_id, max_iterations)` has an independent
judge inspect the branch (branch-aware preview), feeds GAP and CONTRADICTED findings back to
the builder, rebuilds into the same branch and re-judges, up to `max_iterations`. Poll
`get_remediation_status`, then `get_remediation_report`. Confirm before starting: it writes.

## Branch lifecycle

- `list_built_branches()` shows every branch Prodigo authored and its status.
- `set_branch_status(branch, status)` records `judged`, `merged` or `discarded` after the human
  acted in Dev Studio. Prodigo cannot merge or delete a branch.

## Hand-off to Pega's plugin

For a single-rule edit inside the branch, use the `pega-infinity-authoring` tools if that
plugin is installed (ChangeRequest workflow). Say which tool did what.

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

