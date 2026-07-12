# Adapter Architecture

## Overview
Adapters represent the boundary layer of the Coordination Intelligence Layer (CIL). They translate domain-specific events, entities, and relationships into the abstract, domain-neutral contracts (`cil_contracts`) that CIL understands.

## Purpose
To preserve CIL's domain neutrality. By forcing all translation to occur at the edges, the core engines (Graph, Propagation, Decision) remain completely ignorant of whether they are coordinating a physical building model or a microservice architecture.

## Design Principles
- **Translation, Not Logic**: Adapters should not contain coordination logic. Their sole job is mapping.
- **Registration**: Adapters self-register with the Runtime, allowing the system to route events appropriately based on origin.

## Core Responsibilities
1. **Event Ingestion**: Catching domain events (e.g., a webhook from a BIM tool) and converting them into generic CIL triggers.
2. **Entity Mapping**: Converting domain objects (e.g., `IfcWall`) into CIL `Node` contracts, preserving essential metadata (like `discipline`).
3. **Relationship Mapping**: Translating domain relationships into graph dependencies and writing them to the Graph Engine.

## Internal Workflow
1. External system fires an event.
2. The Runtime routes the event to the appropriate Adapter.
3. The Adapter translates the event payload into a `Node`.
4. The Adapter triggers the `PropagationEngine` with the `Node`.

## Examples
The primary example in the repository is the **BIMAdapter** (`cil-runtime/adapters/bim_adapter.py`). It understands IFC relationships and architectural disciplines. When it receives a message that a wall has moved, it translates this into a Node with metadata `{"discipline": "Structural", "ifc_type": "IfcWall"}` and passes it to the core.

## Limitations
The quality of CIL's intelligence is directly constrained by the quality of the Adapter's mapping. If an adapter fails to register a critical dependency in the Graph Engine, CIL cannot predict that dependency's failure.

## Related Topics
- [Domain Neutrality](../concepts/03-domain-neutrality.md)
- [BIM Adapter](02-bim-adapter.md)
- [Contracts](../reference/01-contracts.md)
