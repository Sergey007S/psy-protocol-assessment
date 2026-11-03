Psy MCP Wallet — Spending Budget Not Refunded on Relay Errors
Finding
Static code review identified two error paths in private_transfer where spending limits are authorized but not refunded.
Evidence
File: client_prover/psy_mcp_server/src/private_tools.rs
The wallet authorizes the spending amount before relay validation.
If an explicit relay URL fails validation, the function returns without calling refund().
If no relay is configured, the function also returns without calling refund().
File: client_prover/psy_mcp_server/src/policy.rs
Authorization increases daily, monthly, lifetime and session spending counters.
refund() restores those counters and marks the spend record as refunded.
Impact
Repeated failed attempts may consume the agent's spending limits even though no private transfer is submitted.
This is a policy-accounting defect, not evidence of lost funds or unauthorized transfers.
Verification status
Static code review finding. Runtime reproduction has not yet been performed.
Open question
Should failed private-transfer attempts that stop before transaction submission restore the authorized spending budget?
