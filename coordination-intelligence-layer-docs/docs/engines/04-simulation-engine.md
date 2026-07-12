# Simulation Engine

## Overview
The Simulation Engine provides a secure, isolated environment to run "what-if" scenarios without altering production state or persisting speculative data to the primary graph.

## Purpose
To allow operators, data scientists, and automated test suites to validate policies, analyze theoretical impacts, and generate training data safely.

## Architecture
The engine orchestrates the `PropagationEngine` and `DecisionEngine` as sub-components to run isolated flows.

## Internal Workflow
1. The engine accepts synthetic `Node` definitions, injected mock topologies, or specific `PropagationResult` seeds.
2. It executes propagation against a mock or isolated graph context.
3. It evaluates policies to produce simulated `CoordinationDecision` outputs.
4. The outputs are packaged into a `SimulationResult` contract.

## Outputs and Determinism
Outputs are entirely deterministic based on the current propagation strategy and loaded policies. This enables the creation of highly reproducible test harnesses. Simulation outputs are explicitly tagged in their metadata to ensure they are never confused with production data.

## Runtime Integration
The API Gateway exposes the endpoint `POST /api/simulate`, which proxies requests directly to the Simulation Engine. This is the primary interface for interactive demo flows.

## Use Cases
- **Policy Validation**: Testing a new YAML policy before deploying it to production.
- **Impact Forecasting**: Analyzing the potential blast radius of planned maintenance (e.g., upgrading a core switch in a data center or removing a column in a building).
- **Data Generation**: Generating labeled data representing coordination decisions for future ML experiments.

## Related Topics
- [Scenario Testing Framework](../scenarios/01-scenario-testing-framework.md)
- [Policy and Decision Engine](03-policy-decision-engine.md)
