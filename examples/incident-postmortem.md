# Postmortem: Checkout Notification Delay

On May 20, checkout confirmation notifications were delayed for a subset of customers after the polling worker fell behind during a traffic spike.

## Impact Summary

The incident affected customers who completed checkout between 09:10 and 10:05 UTC. Orders were processed successfully, but confirmation emails and push messages were delayed.

## Timeline

1. 09:10 UTC - Traffic spike begins after campaign launch.
2. 09:18 UTC - Database polling query exceeds the alert threshold.
3. 09:27 UTC - Notification backlog reaches 180,000 pending events.
4. 09:42 UTC - On-call pauses non-critical notification categories.
5. 10:05 UTC - Worker pool catches up after emergency scaling.
6. 10:35 UTC - Incident is resolved and customer support receives the final order list.

## Root Cause

The polling worker scanned the same pending-events index every five minutes. During the campaign spike, each scan competed with checkout writes and became slower than the backlog growth rate.

## What Went Well

- Checkout itself remained available.
- Support received a complete list of affected orders.
- Emergency worker scaling reduced the backlog without data loss.

## What Went Wrong

- The alert fired on query duration but not on customer-visible notification delay.
- The worker had no prioritization for checkout confirmations.
- The dashboard did not show backlog by notification category.

## Corrective Actions

1. Add customer-visible latency alerts.
2. Prioritize checkout confirmation events.
3. Replace polling with event-driven notification delivery.
4. Add backlog-by-category dashboard panels.

## Risks After Mitigation

The event-driven design reduces polling pressure, but duplicate delivery and dead-letter ownership still need explicit controls.

