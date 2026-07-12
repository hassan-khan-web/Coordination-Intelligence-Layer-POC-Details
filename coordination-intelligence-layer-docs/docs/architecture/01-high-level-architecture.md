# High-Level Architecture

## Overview
The Coordination Intelligence Layer (CIL) is built as a layered, modular architecture. It is designed to be highly decoupled, separating domain ingestion, graph persistence, propagation logic, and policy evaluation into distinct components.

## Purpose
This document provides a high-level view of how CIL components are organized and interact within a production environment.

## Architecture Layers
CIL is divided into four primary layers:

1. **Domain Edge (Adapters)**: The boundary layer that interfaces with external systems (e.g., BIM authoring tools, microservice registries). It translates domain-specific events into canonical CIL contracts.
2. **Orchestration Runtime**: The central coordinator that wires the engines together, exposes APIs, and manages the lifecycle of a coordination event.
3. **Core Intelligence Engines**: The mathematical and logical heart of CIL, including the Graph, Propagation, and Decision engines.
4. **Data Persistence**: The backing stores, primarily Neo4j for graph topology and PostgreSQL for metadata and feedback loops.

## Component Architecture

- **Contracts (`cil_contracts`)**: Pydantic models providing the single source of truth for all data passing between components.
- **Runtime (`cil-runtime`)**: A lightweight Python composition manager that instantiates the engines and exposes them to the API gateway.
- **Graph Engine**: Abstraction over Neo4j, persisting nodes and edges.
- **Propagation Engine**: Calculates the blast radius of changes. Dual implementation (Go for production, Python for simulation).
- **Decision Engine**: Evaluates `PropagationResult` against human-authored policies.
- **Explainability Engines**: A suite of analytical engines (Reasoning, Confidence, Timeline, Action Recommendation) that provide transparency into the decision process.

## Data Flow
1. An external system fires an event to the API Gateway.
2. The appropriate **Adapter** translates it into a CIL `Node` and triggers propagation.
3. The **Propagation Engine** queries the **Graph Engine** and computes a `PropagationResult`.
4. The **Decision Engine** evaluates the result against active policies, producing a `CoordinationDecision`.
5. The **Explainability Engines** enrich the decision with causal chains and confidence scores.
6. The **Feedback Loop** captures the final outcome and persists it to PostgreSQL.

## Related Topics
- [Event Lifecycle](02-event-lifecycle.md)
- [Runtime Composition](../runtime/01-runtime-composition.md)
- [Adapter Architecture](../adapters/01-adapter-architecture.md)
