# Feedback Loop

## Overview
The Feedback Loop is a critical mechanism in CIL that closes the circle of coordination by capturing and persisting the actual real-world results of coordination decisions.

## Purpose
To provide a complete audit trail of system behavior, enable transparency, and generate labeled training data for future machine learning models. A decision without a recorded outcome is a lost opportunity for system improvement.

## Architecture
The Feedback Loop is managed by the `FeedbackService` (`cil-runtime/feedback-service/`).

### Responsibilities
- **Event Capture**: Listens for `FeedbackEvent` contracts emitted by adapters, action services, or external systems once a recommended action has been executed or rejected.
- **Outcome Logging**: Persists the final outcome to the PostgreSQL database within the `aeco_metadata` schema.
- **Metrics Generation**: Computes success rates, failure rates, and decision latency.

## Data Persistence
The loop relies on relational data stored in PostgreSQL to map the entire lifecycle:
1. `change_events` table: The original trigger.
2. `propagation_results` table: The predicted blast radius.
3. `coordination_decisions` table: The system's recommended action.
4. `coordination_outcomes` table: The final result (e.g., "Action Accepted", "Manual Override").

## Closed-Loop Intelligence
By storing both the predicted impact and the actual outcome, the system achieves a closed loop. In future iterations (e.g., CIL v0.7), this historical data will be leveraged by the Machine Learning Layer to refine propagation decay curves and adjust policy thresholds automatically based on historical accuracy.

## Related Topics
- [Observability Metrics](01-metrics-and-prometheus.md)
- [Explainable Intelligence v0.2](../explainability/01-explainable-intelligence-v02.md)
