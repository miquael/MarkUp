# RFC: Event-Driven Notification Delivery

The notification platform should move from database polling to event-driven delivery so product messages arrive within seconds, not minutes, while reducing read load on the primary database.

## Goals

- Reduce p99 notification latency from 360 seconds to less than 5 seconds.
- Reduce database polling load by at least 60%.
- Preserve rollback through the full rollout window.
- Keep every event auditable for support and compliance review.

## Baseline Metrics

| Metric | Current | Target |
|---|---:|---:|
| p99 latency | 360s | <5s |
| Database CPU from polling | 30% | <12% |
| Event loss tolerance | None | None |

## Proposed Architecture

Application services publish notification events to Kafka. The notification router consumes the topic, chooses the correct channel, and calls the worker through an internal webhook. The worker sends the message and writes an audit record to Postgres.

The router-to-worker webhook stays inside the private network.

## Delivery Flow

1. Producer creates a notification event after a domain action.
2. Producer writes the event to the transactional outbox.
3. Outbox relay publishes to Kafka.
4. Notification router consumes the event.
5. Worker sends the notification and writes the audit log.

## Message Bus Options

Kafka, SQS, and RabbitMQ can all move events out of the database polling loop. The key selection criteria are replay support, team familiarity, operational cost, and rollback safety.

Decision: choose Kafka because the cluster already exists and replay is required for safe worker restarts.

## Risks

Duplicate delivery can happen during the double-write window. Consumer lag can hide downstream worker failures. Bad event contracts can break older workers.

## Rollout Plan

1. Create the Kafka topic and router skeleton.
2. Add the transactional outbox to producers.
3. Enable router-to-worker delivery behind a feature flag.
4. Run polling and events in parallel for one week.
5. Cut over to event delivery and keep rollback data for two weeks.

## Open Questions

- Should SMS delivery stay synchronous or move through the same event route?
- What is the exact dead-letter queue ownership model?
- How long should the audit table retain full payloads?

