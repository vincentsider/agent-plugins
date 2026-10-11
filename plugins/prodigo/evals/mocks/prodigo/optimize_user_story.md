---
type: fixed
expect:
  story: string
---
# Optimized user story

As a teller, I want to reverse a same-day posted transaction with supervisor approval above 500 EUR, so that customers are never charged twice.

Acceptance criteria:
- AC1 Given a transaction posted today, when I select Reverse, then the reversal case opens.
- AC2 Given an amount above 500 EUR, when I submit, then a supervisor approval assignment is created.
- AC3 Given an approved reversal, when it resolves, then the customer receives a confirmation.

**Priority score:** 7.5
**Priority reasoning:** regulatory exposure and customer impact.
_Grounded in WRBank: ReverseTransaction case type, ApprovalThreshold decision table._
