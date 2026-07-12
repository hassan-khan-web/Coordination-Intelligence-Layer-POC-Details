# Coordination and Propagation

## Overview
Coordination and Propagation are the foundational concepts of the Coordination Intelligence Layer (CIL). They represent how the system understands connectivity and calculates the cascading effects of changes.

## Purpose
To establish a mental model for how CIL evaluates systemic impacts before defining actionable policies.

## Background
In distributed systems or physical building models, elements exist in a web of dependencies. A modification does not happen in a vacuum; it propagates along the edges of a dependency graph.

## Architecture
Propagation is handled by the `PropagationEngine`. Coordination is the higher-level orchestration executed by the `DecisionEngine` based on propagation results.

## Design Principles
- **Ripple Effects**: Every change is a stone dropped in a pond. Propagation measures the ripples.
- **Deterministic Traversal**: Impact should be calculated predictably based on distance and edge weights.

## Internal Workflow
1. A source node experiences an event.
2. The Propagation Engine queries the Graph Engine for connected dependencies.
3. The engine traverses the graph, calculating an `impact_score` for each visited node using a specific strategy (e.g., depth-decay).
4. A `PropagationResult` is compiled, listing all affected nodes and their respective impact scores.

## Data Flow
`Event` -> `PropagationEngine` <-> `GraphEngine` -> `PropagationResult`

## Implementation Details
Propagation is heavily dependent on the chosen `PropagationStrategy`. The default strategy is a deterministic depth-decay, where the impact score decreases the further a node is from the source event.

## Examples
If `Node A` (a wall) supports `Node B` (a beam), and `Node B` supports `Node C` (a duct). An event on `Node A` with an initial impact of 1.0 might propagate to `Node B` with a score of 0.8, and to `Node C` with a score of 0.5.

## Performance Considerations
Graph traversal can become computationally expensive in highly dense graphs. This is why CIL employs a dual-engine architecture: a high-performance Go engine for production streams, and a Python engine for detailed simulations.

## Related Topics
- [Graph Engine](../engines/01-graph-engine.md)
- [Propagation Engine](../engines/02-propagation-engine.md)
- [Dual Propagation Boundary](../architecture/03-dual-propagation-boundary.md)
