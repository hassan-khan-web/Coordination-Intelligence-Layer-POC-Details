# Runtime Composition

## Overview
The CIL Runtime (`cil-runtime/runtime.py`) acts as the lightweight composition and lifecycle manager for the entire Python-based intelligence layer.

## Purpose
To centralize configuration, instantiate the various intelligence engines, and expose a unified interface to API gateways and test harnesses without containing domain logic itself.

## Responsibilities
- **Configuration Loading**: Reads `runtime_config.yaml` to configure connections (e.g., Neo4j) and paths (e.g., policy directories).
- **Engine Instantiation**: Boots up the Graph Engine, Policy Engine, Decision Engine, Propagation Engine, Simulation Engine, and Temporal Engine.
- **Strategy Wiring**: Injects the appropriate `PropagationStrategy` (defaulting to `DeterministicPropagationStrategy`) into the Propagation Engine.
- **Adapter Registry**: Loads and exposes available Adapters (e.g., `BIMAdapter`) to handle incoming domain events.

## Design Choices
- **Composition over Containment**: The runtime exposes engines as attributes (e.g., `runtime.decision_engine.evaluate()`). It acts as a trusted container, not a facade that wraps every engine method.
- **Statelessness**: The runtime itself is stateless, relying on the engines to manage their respective domains (like graph persistence or policy caching).

## Usage
The runtime is designed to be instantiated once per process. For instance, the API Gateway (`cil-runtime/api-gateway/main.py`) creates a singleton `Runtime` instance and exposes its capabilities over HTTP.

## Examples
```python
from cil_runtime.runtime import Runtime

# Initialize the runtime (loads config, boots engines)
runtime = Runtime()

# Access an engine directly
result = runtime.propagation_engine.propagate(event_node)

# Evaluate policies
decision = runtime.decision_engine.evaluate(result)
```

## Related Topics
- [High-Level Architecture](../architecture/01-high-level-architecture.md)
- [Graph Engine](../engines/01-graph-engine.md)
- [API Gateway](03-api-gateway.md)
