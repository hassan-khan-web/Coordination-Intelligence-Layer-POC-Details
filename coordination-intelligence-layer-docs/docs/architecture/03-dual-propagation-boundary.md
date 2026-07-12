# Dual Propagation Boundary

## Overview
The CIL architecture utilizes two distinct implementations for calculating propagation, referred to as the Dual Propagation Boundary.

## Purpose
To balance the competing requirements of high-performance production event processing and highly flexible simulation and testing environments.

## Architecture

### 1. Core Propagation Service (Go)
- **Responsibility**: Source of Truth for Production-scale ripple effect analysis.
- **Implementation**: High-performance graph traversal written in Go.
- **Algorithm**: Optimized for massive graphs (1M+ elements) and low-latency real-time event streams.
- **Integration**: Interacts directly with Neo4j and consumes/produces directly from Kafka topics (`cil-change-events`, `cil-ripple-effects`). Triggered by the Orchestration Service.

### 2. Runtime Propagation Engine (Python)
- **Responsibility**: Source of Truth for Simulations, Local Coordination, and "What-if" Analysis.
- **Implementation**: Python-based `PropagationEngine` utilizing pluggable strategies.
- **Algorithm**: Focused on explainability, easy experimentation with decay curves, and discipline-specific heuristics.
- **Integration**: Drives the Simulation Engine and the local CIL API Gateway.

## Contractual Alignment
Despite differing implementations, both engines are strictly bound by the `cil_contracts`.
- They both ingest `Node` payloads.
- They both output `PropagationResult` payloads.
- They both communicate via the same unified Kafka topics.

This guarantees that a simulation run in Python will yield a structurally identical contract to a production run in Go, ensuring interoperability.

## Summary Table

| Feature | Core Propagation (Go) | Runtime Propagation (Python) |
| :--- | :--- | :--- |
| **Primary Use** | Production / Real-time | Simulation / POC / Testing |
| **Backing Store**| Neo4j (Optimized) | Neo4j (General) / Mock |
| **Output** | `cil-ripple-effects` topic | `PropagationResult` object |
| **Priority** | Performance & Scalability | Explainability & Flexibility |

## Related Topics
- [ADR: Dual-Engine Strategy](../architecture-decisions/002-dual-engine-strategy.md)
- [Propagation Engine](../engines/02-propagation-engine.md)
