# Graph Engine

## Overview
The Graph Engine is the persistence and topology layer of CIL. It is responsible for storing and querying the mathematical representation of the coordination graph.

## Purpose
To provide a fast, queryable representation of dependencies so that the Propagation Engine can rapidly calculate blast radiuses.

## Architecture
The Graph Engine is an abstraction layer that sits on top of a backing store. In production environments, this store is **Neo4j**, chosen for its native graph traversal capabilities.

## Core Responsibilities
- **Persistence**: Exposes methods like `add_node` and `add_edge` to persist topology.
- **Querying**: Provides traversal primitives like `query_dependencies(node_id)` and `query_blast_radius(node_id)`.

## Integration
Adapters (like the `BIMAdapter`) are primarily responsible for feeding the Graph Engine. When an IFC model is parsed, the adapter uses the Graph Engine to write the physical relationships (e.g., a duct passing through a wall) as edges.

## Implementation Details
The engine provides a lightweight schema for node metadata and edge weights. This allows the `PropagationStrategy` to make context-aware decisions (e.g., a "supports" edge might propagate impact differently than a "contains" edge).

## Examples
```python
# Adding an edge provided by the BIMAdapter
runtime.graph_engine.add_edge(
    source="Wall-001",
    target="Beam-050",
    edge_type="SUPPORTS"
)
```

## Performance Considerations
Graph queries are on the critical path for every event. To ensure low latency, the Core Propagation Engine (Go) interacts directly with Neo4j using optimized Cypher queries, bypassing Python overhead for production event streams.

## Related Topics
- [Propagation Engine](02-propagation-engine.md)
- [Dual Propagation Boundary](../architecture/03-dual-propagation-boundary.md)
- [Neo4j and Kafka](../deployment/02-kafka-and-neo4j.md)
