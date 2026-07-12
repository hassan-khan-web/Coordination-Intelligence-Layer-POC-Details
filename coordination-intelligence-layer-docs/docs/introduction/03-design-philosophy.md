# Design Philosophy

## Overview
The design philosophy of the Coordination Intelligence Layer (CIL) defines the foundational principles governing every line of code, architectural decision, and feature implementation in the system.

## Purpose
To provide a guiding framework for developers, architects, and open-source maintainers to ensure that CIL remains scalable, transparent, and domain-agnostic as it evolves.

## Background
CIL was conceived to manage BIM coordination, but it was recognized early that building an A.E.C.O-specific tool would limit its utility and create monolithic codebases. Therefore, a strict philosophical boundary was established.

## Architecture
The architecture is directly downstream of the philosophy:
- **Separation of Concerns:** Core vs. Adapters.
- **Dual Runtime:** High-performance Go vs. Explainable Python.

## Design Principles
1. **Domain Agnosticism is Non-Negotiable**: The core engines must never contain references to domain concepts (like `IfcWall` or `HTTP 500`). Everything is a `Node` or a `Dependency`.
2. **Explainability is a First-Class Citizen**: An orchestration engine that cannot explain its decisions is a liability. Every decision must be traceable.
3. **Data is the API**: Strongly-typed contracts (`cil_contracts`) are the ultimate source of truth, favored over complex RPC protocols.
4. **Simulation over Mutation**: The ability to ask "What If" must be baked into the core, allowing safe experimentation without mutating production state.

## Implementation Details
This philosophy is realized through the Adapter pattern (for domain agnosticism), the Feedback Loop (for continuous learning), and the extensive Explainability Engines (Reasoning, Confidence, Timeline).

## Tradeoffs
- **Complexity at the Edges**: Maintaining strict domain neutrality forces the complexity of translation into the Adapters. Writing an Adapter requires deep knowledge of both the source domain and CIL contracts.
- **Overhead of Explainability**: Generating causal chains and natural language narratives adds latency to the decision-making process.

## Related Topics
- [ADR: Why CIL is Domain-Neutral](../architecture-decisions/001-domain-neutrality.md)
- [Adapter Architecture](../adapters/01-adapter-architecture.md)
- [Explainable Intelligence v0.2](../explainability/01-explainable-intelligence-v02.md)
