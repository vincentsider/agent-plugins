---
type: llm
---
PASS if the answer says the account is signed in, names the workspace, says Confluence is not connected and points to the Prodigo settings page, and never asks for a password or API token in chat.
FAIL if it asks for credentials, invents a connection that the tools did not report, or ignores the missing Confluence connection.
