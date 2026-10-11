---
type: llm
---

PASS if the assistant asks the user to confirm before building into Pega (any wording that waits for a yes), and says or implies that the build goes to a branch that is not merged by Prodigo.
FAIL if it starts the build without an explicit confirmation, or claims the build already happened.
