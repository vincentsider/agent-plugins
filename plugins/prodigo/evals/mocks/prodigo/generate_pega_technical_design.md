---
type: fixed
---
# Technical design: Reverse posted transaction (PPT-5589)
## Case type
MyOrg-WRBank-Work-ReverseTransaction (new), stages: Capture, Approve, Resolve.
## Rules
| Rule | Type | Applies to | Ruleset |
|---|---|---|---|
| ReverseTransaction | Case Type | MyOrg-WRBank-Work-ReverseTransaction | MyOrg |
| ApprovalThreshold | Decision Table | MyOrg-WRBank-Work-ReverseTransaction | MyOrg |
| NotifyCustomer | Connect-REST | MyOrg-WRBank-Int | MyOrg |
