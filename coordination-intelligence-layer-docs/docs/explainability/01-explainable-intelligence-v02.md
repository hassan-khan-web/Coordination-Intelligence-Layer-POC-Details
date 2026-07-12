# Explainable Intelligence v0.2

## Overview
The transition to CIL v0.2 marked a fundamental shift from "black-box" propagation to fully Explainable Coordination Intelligence. Every decision output by the system now includes comprehensive tracing and reasoning.

## Purpose
In enterprise and engineering environments, automated decisions must be justifiable. If CIL recommends halting construction due to an MEP clash, stakeholders must immediately know *why* without consulting log files.

## Architecture
Explainability is achieved through a suite of dedicated analytical engines within the `services/` layer:
1. **Decision Explanation Engine**: Converts raw outputs into human-readable narratives.
2. **Reasoning Chain Engine**: Builds a step-by-step causal chain linking the root event to the final decision.
3. **Confidence Engine**: Assigns a certainty score based on graph completeness and determinism.
4. **Recommended Action Engine**: Translates abstract decisions into concrete, actionable steps.
5. **Timeline Engine**: Captures absolute chronology.

## Data Flow
After the Decision Engine evaluates a policy, the resulting `CoordinationDecision` is routed through the Explainability Engines. They enrich the contract with `reasoning_steps`, `confidence_score`, and an `explanation_id`.

## Explainability APIs
The Orchestration Service exposes these insights via REST:
- `GET /decision/{id}/explanation`
- `GET /decision/{id}/reasoning`
- `GET /decision/{id}/confidence`
- `GET /decision/{id}/narrative`

## The CLI Interface
CIL provides a terminal script (`./cil`) to interact with these engines directly:
- `./cil explain`: Prints the complete causal chain.
- `./cil confidence`: Displays the confidence metrics.

## Implementation Details
The `CoordinationDecision` contract was expanded to hold this new metadata. This ensures that when a decision is persisted or sent via Kafka, its explanation travels with it, providing full context to any consumer.

## Related Topics
- [Causal Chains and Reasoning](02-causal-chains-and-reasoning.md)
- [Confidence Scoring](03-confidence-scoring.md)
- [Orchestration API](../api/01-orchestration-api.md)
