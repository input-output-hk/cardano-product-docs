# Cardano Node gRPC API Go-To-Market Plan

### Executive Summary

The Cardano Node gRPC API provides a robust, high-performance, and language-agnostic interface for developers and services to interact directly with a `cardano-node`. By exposing core node-to-client mini-protocols (LocalStateQuery and LocalTxSubmission) via standardized gRPC endpoints, this project significantly lowers the barrier to entry for developers using languages other than Haskell. It enables the creation of diverse applications and services (dashboards, explorers, wallets, DApps, enterprise integrations) on Cardano without needing external tools for basic node interaction. The goal is to simplify access to full-node capabilities, prioritizing performance, security, and alignment with the node's evolution.

### The 4 Ps

* **Product**: A gRPC and optional JSON-over-HTTP interface integrated directly into the `cardano-node` process. It exposes the `LocalStateQuery` and `LocalTxSubmission` mini-protocols, allowing developers to query real-time ledger state and submit transactions directly from various programming languages. It leverages gRPC and Protobuf for efficient, type-safe communication and includes features like dynamic configuration reloading and monitoring endpoints.
* **Price**: Open-source, freely available to the Cardano community, aligning with Cardano's open architecture philosophy. 
* **Place**: Protobuf definitions published as part of a Cardano Improvement Proposal (CIP). Client stubs/generation instructions for key languages will be provided. Integrated directly in `cardano-node` repository. Documentation will reside on developers.cardano.org.
* **Promotion**: Strategy focuses on developer and enterprise engagement. Includes detailed technical documentation and tutorials, client generation guides and examples, technical blog posts, workshops/webinars demonstrating usage and benefits, announcements via developer forums, social media (Twitter/X, etc.), and engagement within Cardano community channels (e.g., Discord, Intersect). Collaboration with ecosystem partners to showcase integration possibilities.

### Vision

* **Language Agnosticism**: Expand the Cardano ecosystem by making direct node interaction accessible from any language with RPC support.
* **Standardized Integration**: Leverage the mature gRPC framework and Protobuf schema definition language for type-safe, efficient communication.

### Product Positioning

#### Problems Cardano Node gRPC API Solves

* **Language Barrier:** Eliminates the need for Haskell expertise or reliance on specific community tools for direct node interaction, opening development to a wider range of programmers and technology stacks (e.g., Java, Python, Go, Rust, Node.js).
* **Dependency on External Tools:** Reduces reliance on side-car services (like db-sync, Ogmios, submit-api) for basic node queries and transaction submission, simplifying deployment and potentially improving performance.
* **Standardization Need:** Provides a standardized, well-defined API using industry standards (gRPC, Protobuf) for node communication, improving developer experience and integration robustness.
* **Direct Node Access Requirement:** Offers direct access to real-time node data (LocalStateQuery) and transaction submission (LocalTxSubmission) integrated within the node process itself.
* **Transaction Submission Feedback:** Provides immediate and precise transaction validation feedback directly from the node via RPC errors.

### Competitive Advantage

Compared to interacting via Haskell libraries, or existing sidecar solutions:

* **Direct In-Process Integration:** Runs within the `cardano-node` process, ensuring minimal overhead and direct communication compared to separate services.
* **Language Agnosticism:** Officially supports interaction from multiple programming languages via gRPC and Protobuf definitions.
* **Standardization & Tooling:** Leverages the mature gRPC ecosystem and Protobuf for type safety, performance, and wide tooling support.
* **Performance:** Aims for efficient handling of concurrent requests without significantly degrading node performance, potentially offering lower latency for direct queries/submissions compared to layered solutions.
* **Simplified Deployment:** Reduces the components needed for basic node interaction compared to deploying and managing separate sidecar services.

### Target Audience

* **Third-Party Developers:** Building dashboards, explorers, wallets, DApps, or tooling requiring real-time node data and transaction submission capabilities.
* **Enterprise Teams:** Integrating Cardano functionality into diverse technology stacks (e.g., Java microservices, Python data pipelines, Go applications).

### Channel Strategy

* **Developer Portals:** Host primary documentation, tutorials, client stub generation guides, and examples.
* **GitHub:** Publish Protobuf files, potentially client library code/examples, and manage issue tracking/feedback.
* **Technical Blogs & Content:** Publish deep-dive articles explaining architecture, usage patterns, and benefits.
* **Workshops & Webinars:** Conduct live sessions demonstrating setup, integration, and specific use cases for developers and enterprise teams.IOG Academy. 
* **Community Channels:** Engage in developer forums, Discord servers (e.g., Cardano Developers, IOG Technical Community), and relevant Intersect workgroups for Q&A and feedback.
* **Social Media:** Announce releases, features, and content via channels like Twitter/X.
* **Ecosystem Collaboration:** Work with wallet providers, explorer teams, DApp developers, and tooling providers to encourage adoption and integration.

### Product Launch Plan

**Core Messaging:**
* "Interact directly with Cardano nodes from any language using the official gRPC API."
* "High-performance, standardized access to Cardano node queries and transaction submission."
* "Simplify your Cardano integration: Less reliance on external tools, more direct control."

#### Launch Strategy:

The launch will follow a phased approach focused on iterative development and community feedback:

1.  **Internal Development & Testing:** Focus on building the core functionality and ensuring stability through rigorous internal testing and prototyping.
2.  **Early Adopter Program:** Engage a select group of target users (developers, enterprise teams) for early access, feedback collection, and validation of core use cases. Refine the API and documentation based on their input.
3.  **Public Release:** Announce general availability alongside comprehensive documentation, client generation tools/guides, and usage examples. Promote through developer channels and community engagement.
4.  **Post-Launch Iteration:** Actively monitor adoption, gather broader community feedback, and prioritize ongoing improvements, bug fixes, and potential feature enhancements based on user needs and strategic goals.

### Metrics and KPI

* **Adoption:**
    * Number of distinct projects/teams integrating the gRPC API.
    * Downloads/usage of official Protobuf files and client libraries/stubs.
    * CIP discussion volume and community engagement.
* **Performance:**
    * Average and p95 latency for key LocalStateQuery methods.
    * Average latency for SubmitTransaction requests.
    * Measured CPU and memory overhead added to `cardano-node`.
* **Stability & Reliability:**
    * Uptime of the gRPC service in monitored deployments.
* **Community Feedback:**
    * Qualitative feedback on ease of use, documentation quality, feature completeness via forums, GitHub issues, surveys.

### Risk Management

| Dependency/Risk             | Description                                                                 | Mitigation Strategy                                                                                           |
| :-------------------------- | :-------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------ |
| Performance Impact          | High RPC load could potentially impact node's primary functions.       | Make RPC optional; implement efficient request handling; conduct load testing; monitoring. |
| .proto Versioning           | Breaking changes in API definitions can disrupt client applications.   | Adhere strictly to backward compatibility rules; clear versioning & comms plan. |
| Dynamic Reload Complexity   | Implementing reliable hot-reloading without disrupting the node is complex. | Careful design; implement robust triggers/fallback logic; thorough testing of reload scenarios. |
| Adoption                    | Developers might prefer existing solutions or face integration hurdles.       | Strong documentation, client examples, active community support, highlight benefits (performance, simplicity, standardization). |

### Roadmap

This roadmap outlines the key stages and tasks for delivering the Cardano Node gRPC API.

* **Phase 1: Prototype & Design Finalization**
    * Define initial API structure using draft `.proto` definitions for core protocols and create a working Proof-of-Concept (PoC) integrated with `cardano-node`. Coordinate with txPipe and blinklabs.
    * Establish the initial benchmarking setup and framework to measure performance impact.

* **Phase 2: Core Implementation & Security**
    * Build the production-ready gRPC server implementation within the `cardano-node`, including configuration options (flags/config files).
    * Develop comprehensive automated tests (unit, integration, basic concurrency) to ensure reliability.

* **Phase 3: JSON Interface, Client Support & Documentation**
    * Implement the JSON/HTTP interface, potentially using tools like gRPC-Gateway.
    * Publish finalized `.proto` files and provide clear instructions, scripts, or generated stubs for client creation in key languages (e.g., Go, Rust, Python).

* **Phase 4: Monitoring, Optimization & Maintenance**
    * Implement the `/metrics` endpoint for Prometheus-compatible monitoring and conduct performance analysis/optimization based on benchmarking.
    * Formalize and enact the API versioning strategy, establish ongoing maintenance procedures, and address bug fixes.
    * Investigate community demand and feasibility for exposing additional node mini-protocols via the gRPC interface.
    * Create detailed documentation covering setup, configuration, API usage, security, dynamic reload, and provide example client applications.