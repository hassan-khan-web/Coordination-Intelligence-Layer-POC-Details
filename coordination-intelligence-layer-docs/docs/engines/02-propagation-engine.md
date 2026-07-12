# Propagation Engine

## Overview
The Propagation Engine is the computational heart of CIL. It determines how a localized event ripples outward through the dependency graph.

## Purpose
To calculate an objective, mathematical `impact_score` for every node connected to a source event, producing a `PropagationResult` that the Decision Engine can evaluate.

## Architecture
CIL uses a Dual-Engine Strategy for propagation:
1. **Core Propagation (Go)**: High-performance, optimized for massive models and real-time Kafka streams.
2. **Runtime Propagation (Python)**: Optimized for simulation, policy iteration, and explainability.

## Internal Workflow
When triggered, the Propagation Engine:
1. Receives a source `Node`.
2. Invokes its injected `PropagationStrategy`.
3. The strategy queries the Graph Engine to traverse edges.
4. The strategy returns a map of Node IDs to impact scores.
5. The engine wraps this into a `PropagationResult` contract.

## Implementation Details
The current implementation relies on a **Deterministic Depth-Decay Strategy**. The impact score decreases by a configurable factor at each hop from the source node. This ensures predictable, explainable outcomes.

## Extensibility
The engine is designed to be strategy-agnostic. Future implementations could inject Machine Learning models or probabilistic strategies (e.g., Markov chains) without altering the engine's core logic.

## Examples
An event with an initial severity of 1.0 occurs.
- Hop 1: Connected nodes receive an impact of 0.8.
- Hop 2: Connected nodes receive an impact of 0.6.
- The `PropagationResult` packages these nodes and scores for the policy engine.

## Related Topics
- [Graph Engine](01-graph-engine.md)
- [Coordination and Propagation](../concepts/01-coordination-and-propagation.md)
- [Policy and Decision Engine](03-policy-decision-engine.md)
