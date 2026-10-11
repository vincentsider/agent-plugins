---
type: fixed
expect:
  story_key: /^[A-Z]+-\d+$/
---
# PPT-5589: Teller reverses a posted transaction
As a teller, I want to reverse a transaction posted today so that a customer is not charged twice.
Acceptance criteria:
- AC1 Only transactions posted today can be reversed.
- AC2 A reversal requires supervisor approval above 500 EUR.
- AC3 The customer receives a confirmation.
