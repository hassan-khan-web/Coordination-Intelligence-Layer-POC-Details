# What is the Coordination Intelligence Layer?

## Overview
The Coordination Intelligence Layer (CIL) is a robust, domain-neutral orchestration and coordination middleware. It operates as the connective tissue for distributed systems, analyzing how an isolated event or failure ripples through complex, interdependent networks.

## Purpose
In modern engineering and architectural ecosystems—ranging from BIM workflows to microservice mesh deployments—changes are rarely isolated. Modifying a structural column impacts HVAC ducts, plumbing, and architectural aesthetics. CIL exists to automatically predict, evaluate, and orchestrate the necessary actions to resolve these cross-domain ripple effects. 

## Background
Historically, coordination across disciplines (e.g., Structural vs. MEP engineering) relied on manual review and static clash detection. These processes were reactive, time-consuming, and difficult to audit. CIL replaces this paradigm with a proactive intelligence layer that computes impacts in real-time using graph traversal, policy evaluation, and explainable decision-making.

## Problem Statement
When an event occurs in a highly coupled system, traditional architectures struggle to determine the "blast radius" without hardcoded domain logic. 
- How do we know what will break?
- What policy governs the required response?
- How do we ensure decisions are transparent and justifiable?

## Architecture
CIL sits between domain-specific systems (like Revit or IoT gateways) and execution systems. It receives abstracted events, models them on a dependency graph, calculates propagation, evaluates human-authored policies, and emits coordination decisions.

## Design Principles
- **Domain Neutrality:** Core CIL logic operates entirely on abstract nodes and edges. It does not know what a "Wall" or a "Database" is.
- **Explainability First:** Every decision must be traceable to a specific policy, reasoning chain, and confidence score.
- **Predictable Outcomes:** Core propagation logic prioritizes deterministic, trackable traversal strategies.

## Internal Workflow
1. Event ingested via an Adapter.
2. Abstract Node/Dependency representation constructed.
3. Propagation Engine calculates impact depth and scope.
4. Decision Engine evaluates outcomes against YAML policies.
5. Explainable Coordination Decision is emitted and persisted.

## Component Interaction
CIL relies on distinct, decoupled Engines (Graph, Propagation, Decision, Simulation, Temporal). They communicate primarily via strongly-typed Pydantic contracts and Kafka event streams (`cil-change-events`, `cil-ripple-effects`).

## Examples
*An A.E.C.O scenario:* An IFC event indicates a load-bearing wall is removed. CIL calculates that an HVAC duct relies on that wall, applies a structural policy, and recommends an "EMERGENCY_REVIEW" action with an attached reasoning chain.

## Limitations
CIL does not perform the domain action itself; it only recommends and orchestrates. It relies entirely on Adapters to correctly map physical or logical dependencies into its graph.

## Future Improvements
Transitioning to machine learning-driven propagation strategies that use historical feedback loop data to refine impact scores contextually.

## Related Topics
- [Design Philosophy](03-design-philosophy.md)
- [Coordination and Propagation](../concepts/01-coordination-and-propagation.md)
- [High-Level Architecture](../architecture/01-high-level-architecture.md)
