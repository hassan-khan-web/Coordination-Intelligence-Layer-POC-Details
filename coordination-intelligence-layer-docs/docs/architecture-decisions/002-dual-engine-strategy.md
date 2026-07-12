# ADR 002: Dual-Engine Strategy for Propagation

## Context
Graph traversal and impact calculation (Propagation) are computationally expensive. In production environments processing real-time Kafka streams (`cil-change-events`), the system must handle massive graphs (1M+ nodes) with minimal latency. Conversely, for local testing, "what-if" simulations, and policy tuning, developers need a highly flexible, explainable engine where they can easily tweak decay curves.

## Decision
We implemented a Dual-Engine Strategy for propagation:
1. **Core Propagation (Go)**: A high-performance service optimized for real-time Kafka streams and deep Neo4j integration. This is the source of truth for production.
2. **Runtime Propagation (Python)**: A lightweight, pluggable Python engine optimized for rapid iteration, simulation, and deep explainability tracing. This is the source of truth for testing and simulations.

## Alternatives Considered
1. **Python Only**: While highly explainable and easy to iterate, Python's overhead made it unsuitable for real-time traversal of massive graphs under heavy load.
2. **Go Only**: Writing everything in Go would ensure performance but would slow down policy iteration and make the "what-if" simulation endpoints much harder to build and maintain for data scientists.

## Consequences
- **Positive**: We achieve both massive scale in production and rapid flexibility in simulation.
- **Negative**: We must rigorously maintain the contractual boundary. Both engines *must* consume the exact same `cil_contracts` and produce the exact same `PropagationResult` schemas to guarantee interoperability.

## Tradeoffs
We traded codebase simplicity (maintaining two implementations of propagation) for operational excellence across both production and simulation environments.

## Related Topics
- [Propagation Engine](../engines/02-propagation-engine.md)
- [Dual Propagation Boundary](../architecture/03-dual-propagation-boundary.md)
