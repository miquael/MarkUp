# Technical Spec: Account Health API

The Account Health API returns a read-only summary of account risk, usage, support activity, and billing state for support leads preparing for customer calls.

## Status

The spec is accepted for the next support tooling sprint. Implementation is blocked on access-review approval for billing fields.

## Requirements

- Return a single account health payload by `account_id`.
- Include usage summary, open ticket count, escalation status, and billing risk.
- Require the `support-lead` role for billing fields.
- Keep the API read-only in the first release.
- Respond in under 300ms at p95 for accounts with fewer than 500 tickets.

## Non-Goals

- Editing account notes.
- Updating support ownership.
- Changing billing state.
- Building a customer-facing dashboard.

## API Contract

The API exposes `GET /api/account-health/:account_id`. The response is JSON and should stay stable for the first support tooling release.

## Data Sources

The endpoint reads from usage metrics, recent support tickets, escalation state, and billing state. Billing fields are omitted unless the requester has the `support-lead` role.

## Access Control

Access must be checked before querying billing state. The response should include `billing_risk: null` when the requester lacks access.

## Implementation Notes

1. Add the response type and contract tests.
2. Build the API route with role checks.
3. Add cached reads for usage metrics.
4. Add support-ticket aggregation.
5. Add dashboard integration tests.

## Risks

The main risks are overexposing billing information, slow ticket aggregation for large accounts, and contract drift between API and frontend.

## Open Questions

- Should billing risk be hidden or returned as `null` when access is denied?
- Should the API include links to recent tickets or only aggregate counts?
- How long should usage metrics be cached?
