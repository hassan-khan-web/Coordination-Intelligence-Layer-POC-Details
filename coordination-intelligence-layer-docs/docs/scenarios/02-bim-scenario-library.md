# BIM Scenario Library

## Overview
The BIM Scenario Library is a collection of pre-configured, realistic events designed to test and demonstrate the capabilities of the Coordination Intelligence Layer in an A.E.C.O context.

## Purpose
To provide a deterministic "gold standard" for coordination behavior, ensuring that changes to core engines or policies do not introduce regressions.

## Architecture
The library is located in `cil-runtime/scenarios/` and consists of 15 deterministic JSON scenarios. These scenarios serve as reproducible templates for the `SimulationEngine`.

## The 15 Core Scenarios
The library covers critical structural and MEP events:
1. Structural Wall Removal
2. Beam Modification
3. HVAC System Modification
4. MEP Clash Detection
5. Mechanical Equipment Failure
6. Slab Modification
7. Column Removal
8. Lighting Change
9. Sprinkler Modification
10. Elevator Maintenance
11. Foundation Change
12. Roof Modification
13. Glazing/Thermal Change
14. Plumbing Leak
15. Security System Upgrade

## Automated Validation
The test suite (`cil-runtime/tests/test_bim_scenarios.py`) provides end-to-end validation of these scenarios.

**Execution Flow:**
1. **Ingest**: The `BIMAdapter` parses the JSON scenario.
2. **Propagate**: The `PropagationEngine` calculates the blast radius.
3. **Decide**: The `DecisionEngine` evaluates policies.
4. **Verify**: Pytest asserts the outcomes.

## Assertions
Tests verify that the correct nodes are identified within the blast radius and that the policy engine triggers the expected coordination action (e.g., confirming that a "Pump Failure" triggers an "EMERGENCY_MAINTENANCE" action).

## Related Topics
- [Simulation Engine](../engines/04-simulation-engine.md)
- [BIM Adapter](../adapters/02-bim-adapter.md)
