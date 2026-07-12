# CLI Overview

## Overview
The Coordination Intelligence Layer operates primarily as a headless, programmatic orchestration engine. To interact with it, developers and operators use the dedicated CIL Command Line Interface (`cil` script).

## Purpose
To provide a fast, scriptable method for interrogating the system, viewing explainability traces, and running simulations without relying on experimental web UIs.

## Architecture
The CLI is a Python script located at the root of the repository (`./cil`). It communicates with the Orchestration Service and API Gateway over HTTP REST to fetch data and trigger workflows.

## Core Commands

### `cil explain`
Interrogates a specific coordination decision to print a complete causal chain. It answers "WHY THIS DECISION?" culminating in the final action, confidence score, and recommendations.
*Example:* `./cil explain --decision-id 12345`

### `cil timeline`
Streams the chronological engine events for a specific decision, showing the exact pipeline flow from ingestion to explanation.
*Example:* `./cil timeline --decision-id 12345`

### `cil actions`
Lists the specific actionable tasks recommended by the system and the explicit reasoning for each.
*Example:* `./cil actions --decision-id 12345`

### `cil confidence`
Displays the system's confidence metrics, showing how the score was calculated (e.g., graph completeness, deterministic propagation).
*Example:* `./cil confidence --decision-id 12345`

## Best Practices
- Use the CLI for all automated testing and CI/CD validation.
- Pipe CLI output to `jq` for advanced JSON parsing when building automation scripts.

## Common Errors
- **Connection Refused**: Ensure the Orchestration Service (FastAPI) is running on port `8001`.
- **Decision Not Found**: Ensure the provided decision ID exists in the database.

## Related Topics
- [Explainable Intelligence v0.2](../explainability/01-explainable-intelligence-v02.md)
- [Scenario Testing Framework](../scenarios/01-scenario-testing-framework.md)
