# ADR 001: Why CIL is Domain-Neutral

## Context
When designing the Coordination Intelligence Layer (CIL), the initial use case was resolving clashes and managing changes in Building Information Modeling (BIM) for the Architecture, Engineering, Construction, and Operations (A.E.C.O) sector. The temptation was to build a system that natively understood `IfcWall`, `IfcBeam`, and `MEP` systems.

## Decision
We decided that the core CIL engines and contracts must remain strictly domain-neutral. The core system understands only abstract `Nodes`, `Dependencies`, and `Impacts`. All domain-specific knowledge must be pushed to edge `Adapters`.

## Alternatives Considered
1. **BIM-Native Core**: Building a system where the graph engine natively schemas IFC types. This would allow faster initial development for A.E.C.O but would make the system useless for other domains (e.g., microservice dependency mapping).
2. **Hybrid Core**: Allowing a mix of generic nodes and domain-specific subclasses in the core runtime.

## Consequences
- **Positive**: CIL can be applied to any domain that can be modeled as a dependency graph. The core logic remains clean, mathematical, and focused entirely on propagation and policy execution.
- **Negative**: It pushes significant complexity onto the developers building Adapters. The Adapter must perfectly translate domain complexity into abstract nodes, and translate abstract decisions back into domain actions.

## Tradeoffs
We traded ease of initial development for long-term flexibility and architectural purity. The existence of the `BIMAdapter` proves the viability of this approach, successfully mapping complex IFC relationships into the neutral core.

## Related Topics
- [Design Philosophy](../introduction/03-design-philosophy.md)
- [Adapter Architecture](../adapters/01-adapter-architecture.md)
