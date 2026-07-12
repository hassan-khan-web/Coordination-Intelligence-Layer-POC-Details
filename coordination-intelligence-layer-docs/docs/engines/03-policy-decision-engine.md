# Policy and Decision Engines

## Overview
The Policy Engine and Decision Engine work in tandem to evaluate the computed blast radius of an event and determine the appropriate automated response.

## Purpose
While the Propagation Engine calculates *what* is affected (the math), the Decision Engine calculates *what to do about it* (the business logic).

## Architecture
- **Policy Engine**: Responsible for loading, parsing, and managing human-authored YAML policies from disk (e.g., configured via `policy_engine.policy_path`).
- **Decision Engine**: Takes a `PropagationResult` and evaluates it against the active policies provided by the Policy Engine, emitting a `CoordinationDecision`.

## Policy Format
Policies are threshold or rule-based conditions expressed in YAML. They can leverage the metadata injected by Adapters.
For BIM, conditions often include `discipline_eq` (e.g., Structural), `element_type_eq` (e.g., IfcWall), or impact thresholds.

Example Policy Logic:
*If `impact_score > 0.8` AND `discipline == Structural`, then action is `REVIEW_REQUIRED`.*

## Internal Workflow
1. The Decision Engine receives a `PropagationResult`.
2. It evaluates candidate policies in priority order.
3. Upon a match, it generates a `CoordinationDecision` containing the `action`, `affected_nodes`, and `confidence`.
4. It passes the decision to the Explainability Engines to append a human-readable `reasoning` trace.

## Explainability Integration (Priority 2)
Every decision exposes exactly which policy was triggered and the specific elements involved. The resulting causal chain guarantees that project stakeholders can audit why an action was recommended.

## Future Improvements
Transitioning from static YAML policies to dynamically versioned policies, or implementing automated suggestion generation based on analytics.

## Related Topics
- [Propagation Engine](02-propagation-engine.md)
- [Explainable Intelligence v0.2](../explainability/01-explainable-intelligence-v02.md)
