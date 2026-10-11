---
name: house-style
description: Use when the user wants stories to follow their company's own format or method, asks Prodigo to learn from existing Confluence pages or Jira stories, wants to see or correct the learned style, or asks to publish a story to Confluence. Not for writing a single story (story-quality).
---

# House style

Prodigo learns the client's story format (sections, header table, field tables) and their
reasoning method from three to five of their own stories, then applies both to every new
story and scores conformance.

## Learn

`learn_house_style(page_ids | cql | jira_keys | jira_jql | pasted, limit)`:
- Confluence sources need the Confluence connection; Jira sources need the Jira connection;
  `pasted` works with nothing connected.
- Prefer three to five stories from different journeys. Confirm the source list with the user
  before learning; learning overwrites nothing the user corrected earlier.

## Inspect and correct

- `get_house_style()` shows what was learned. Summarise format and method separately.
- `correct_house_style(corrections)` with a mapping of field to corrected value, for example
  `{"header_table_fields": ["Case", "Role", "User story", "When", "Then", "Priority"]}`.
  The user's words win over the learned text.
- `forget_house_style()` deletes it. Confirm first; this is irreversible.

## Publish

`publish_story_to_confluence(story_markdown, title, linked_issue, parent_id, space_key)`
writes a page in the learned format. Always confirm the space and title first.

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

