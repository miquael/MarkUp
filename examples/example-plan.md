# Plan: Launch Account Health Dashboard

The support team needs a compact account health dashboard so escalation owners can see risk, usage, and open issues before joining a customer call.

## Goals

- Show account risk, usage, and support status in one page.
- Keep the first release read-only.
- Ship behind a feature flag for support leads.

## Current Status

The dashboard is proposed for the next support tooling sprint. Data contracts are accepted, but the rollout is still blocked on final access review.

## Dashboard Flow

Support opens an account page. The frontend calls the dashboard API. The API reads usage metrics, recent tickets, and billing state, then returns a summarized account health payload.

## Implementation Steps

1. Define the account health response contract.
2. Build the read-only API route.
3. Add the dashboard view behind a feature flag.
4. Test with five support leads.
5. Roll out to the full support team.

## Risks

The main risk is exposing billing state to users who should only see support data. The mitigation is to require the support-lead role before returning billing fields.

## Recommendation

Start with a read-only dashboard and defer editing workflows until the team validates the account health summary.

## Appendix: Deferred Editing Workflow

Editing account notes, support ownership, or billing state is intentionally out of scope for the first release. Those workflows need audit logging, permission review, and rollback behavior before they are safe to expose.
