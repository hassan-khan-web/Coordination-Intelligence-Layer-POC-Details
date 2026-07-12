# Contracts (cil_contracts)

## Overview
The `cil_contracts` package acts as the **single source of truth** for data passing through the Coordination Intelligence Layer. It defines the canonical data models used by all engines, adapters, and services.

## Purpose
To ensure deterministic serialization and deserialization across distributed systems (Python APIs, Go microservices, Kafka topics), eliminating redundant schema definitions.

## Core Contracts
- **`Node`**: Represents a generic entity in the coordination graph.
- **`Dependency`**: Represents a directed edge or relationship between two Nodes.
- **`PropagationResult`**: The output of the Propagation Engine, containing the blast radius, affected nodes, and compatibility fields for both Go and Python environments.
- **`CoordinationDecision`**: The output of the Decision Engine. It encapsulates the recommended action, affected nodes, confidence scores, and explainability reasoning.
- **`FeedbackEvent`**: Used by the Feedback Loop to log the final outcome of an executed action.

## Schema Versioning
Every contract includes a `schema_version` class attribute.
- Functions like `get_schema_version` and `model_json_schema()` are exposed.
- Versioning ensures backward compatibility when consuming messages from Kafka, allowing event replay even as contracts evolve.

## Implementation Details
Contracts are implemented using `pydantic.BaseModel` in Python, providing robust runtime validation and JSON schema generation. They are strictly domain-neutral; they do not contain fields like `wall_thickness` or `http_status`.

## Examples
```python
from cil_contracts import Node

# Creating a Node contract
node = Node(
    id="uuid-1234",
    metadata={"type": "generic"}
)
```

## Validation
Unit tests in the repository assert the presence of `schema_version` and validate round-trip JSON serialization for every contract.

## Related Topics
- [Domain Neutrality](../concepts/03-domain-neutrality.md)
- [Adapter Architecture](../adapters/01-adapter-architecture.md)
