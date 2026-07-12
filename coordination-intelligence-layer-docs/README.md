# Coordination Intelligence Layer (CIL) Documentation

Welcome to the official documentation for the Coordination Intelligence Layer (CIL).

CIL is a domain-neutral orchestration and coordination middleware designed to attach to external systems—such as Building Information Modeling (BIM) pipelines, microservices, and IoT networks—to provide dependency-aware propagation, systemic reasoning, and adaptive orchestration.

This documentation serves as the comprehensive engineering manual for understanding, deploying, and extending CIL.

## Navigating the Documentation

The documentation is structured to guide you from core concepts to deep implementation details. We recommend reading in the following order:

1. **[Introduction](docs/introduction/01-what-is-cil.md)** - Understand what CIL is and why it was built.
2. **[Getting Started](docs/getting-started/01-prerequisites.md)** - Set up your local environment.
3. **[Concepts](docs/concepts/01-coordination-and-propagation.md)** - Learn the domain-neutral ideas that power CIL.
4. **[Architecture](docs/architecture/01-high-level-architecture.md)** - Explore the layered system architecture.
5. **[Engines](docs/engines/01-graph-engine.md)** - Dive deep into each internal engine.
6. **[Adapters](docs/adapters/01-adapter-architecture.md)** - Understand how CIL integrates with domain-specific systems.

## Documentation Philosophy

This repository is organized around concepts, architecture, workflows, and engineering decisions, rather than codebase folders. Each topic explains:
- What a component is.
- Why it exists and why it was designed this way.
- How it communicates and fails.
- What tradeoffs were considered.

## Quick Links
- **[Architecture Decision Records (ADRs)](docs/architecture-decisions/001-domain-neutrality.md)**
- **[Contracts Reference](docs/reference/01-contracts.md)**
- **[CLI Reference](docs/cli/01-cil-cli-overview.md)**
- **[API Reference](docs/api/01-orchestration-api.md)**
