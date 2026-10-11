---
name: migration-planner
description: Use when the user wants a reviewable plan before building into Pega, needs per-rule create/update/skip/conflict verdicts against an existing application, wants to approve a plan and run it durably, or wants to migrate a legacy application onto a Pega target. Not for a quick build of a fresh design (build-to-branch).
---

# Migration planner

## Plan, then approve, then build

1. `start_migration_plan(design, branch, origin_ruleset, applies_to_class, application, app_id)`
   reconciles every proposed rule against the target application and orders them into
   dependency waves. It authors nothing and pauses for approval.
2. `get_plan_report(plan_id)` shows per-rule verdicts (create, update, skip, conflict), the
   waves, and the rules that need the user's input. Walk the user through conflicts.
3. After an explicit yes, `approve_plan(plan_id)` starts the durable build into the branch.
   Poll `get_plan_status(plan_id)`; the execution survives restarts.
4. For rules that need input, `provide_rule_inputs(plan_id, rule_inputs)` with a JSON array
   of `{"rule_name", "endpoint_url", ...}`. It builds into the same branch, never a new one.

## Legacy to Pega

`start_migration(source_app_url, target_branch, role_label, max_stories, max_screens,
target_preview_url, dry_run)` documents the SOURCE application read-only, turns each
recovered story into a design, builds onto the TARGET branch and, with a preview URL, judges
target against source. Poll `get_migration_status`, then `get_migration_report`. Confirm
before starting: it writes to the target branch.

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

