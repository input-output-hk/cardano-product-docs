# Cardano Node gRPC API 

Product Requirements Document[^1]

## 1\. Executive Summary

### 1.1. Objective

The Cardano Node RPC API aims to provide a robust, and language-agnostic interface for developers and services to interact directly with a `cardano-node`. By exposing core node-to-client mini-protocols via RPC endpoints, this project lowers the barrier to entry for developers using languages other than Haskell, enabling them to build applications and services on Cardano without relying on external tools or third-party libraries for basic node interaction.

The ultimate goal is to simplify access to Cardano's full-node capabilities while prioritizing performance, security, and alignment with the node's ongoing evolution.

### 1.2. Target audience

* **Third-Party Developers:** Building dashboards, explorers, wallets, DApps or tooling requiring real-time node data and transaction submission capabilities.  
* **Enterprise Teams:** Integrating Cardano functionality into diverse technology stacks (e.g., Java microservices, Python data pipelines, Go applications).  
* **DApp & Protocol Developers:** Seeking a direct, standardized node interface in their preferred programming languages.

### 1.3. Vision

* **Language Agnosticism:** Expand the Cardano ecosystem by making direct node interaction accessible from any language with RPC support.  
* **Standardized Integration:** Leverage the mature gRPC framework and Protobuf schema definition language for type-safe, efficient communication.

### 1.4. Strategic fit

* **Open & Modular Architecture:** Aligns with Cardano's philosophy of providing open, composable components.  
* **Ecosystem Growth:** Lowers the barrier for new developers and diverse applications integrating with Cardano.  
* **Improved Developer Experience:** Leverages widespread gRPC and Protobuf tooling, reducing the learning curve for node interaction.

## 2\. Goals and non-goals

### 2.1. Goals

* **G1: Expose Node-to-Client Protocols:** Implement RPC services that expose the full functionality of the `LocalStateQuery` and `LocalTxSubmission` node-to-client mini-protocols.  
* **G2: In-Process Implementation:**   Provide the RPC interface as a feature **within** the `cardano-node` process itself, rather than as a separate sidecar service. This ensures minimal overhead, direct communication with the node's internal processes, and simpler deployment and configuration. Sidecars already exist.  
* **G3: Direct Transaction Submission:** Enable transaction submission directly to the node from applications written in various languages via a dedicated RPC endpoint.  
* **G4: Cardano Improvement Proposal that defines the interface (i.e. .proto files)**  
   Publish official `.proto` files and provide clear instructions or generated client stubs for key languages (initially Go, Rust, Python, Java, Kutlin, Node.js), along with usage examples.  
* **G6: Dynamic Configuration Reload:** Allow operators to enable, disable, and update specific RPC server configurations (e.g., TLS certificates, port) without requiring a full `cardano-node` restart.  
* **G7: Optional JSON Interface:** Provide a JSON-over-HTTP interface equivalent to the RPC services.  
* **G8: Monitoring & Observability:** Expose operational metrics via a standard endpoint (e.g., `/metrics`) in Prometheus format.

### 2.2. Non-goals

* **NG1: Advanced Application-Level Auth:** User-specific Access Control Lists (ACLs) or token-based authentication (e.g., JWT, OAuth2) are out of scope. Trust assumptions mirror the existing node-to-client socket interface.  
* **NG2: Ledger Rule Modification:** This project does not alter Cardano's consensus, network or ledger rules.  
* **NG3: Integrated Caching/Database:** The RPC layer itself will not implement dedicated caching or persistent storage for query results.  
* **NG4: Built‑in Transport Security (TLS/mTLS):** Native TLS or mTLS does not need to be  delivered. Deployments should rely on a standard TLS‑terminating reverse proxy to handle encryption instead.

## 3\. User Journeys

### 3.1. Querying Live Node Data (Any Client Application)

* **Scenario:** An application needs up‑to‑the‑second ledger information (e.g., current epoch, stake distribution, proposals, etc).  
* **Actions:**  
  1. Start **`cardano‑node`** with the RPC server enabled, either through flags or configuration files  (e.g., `--enable-grpc or enableRPC:”True”`).  
  2. Generate RPC client stubs from the official **`.proto`** files in the application’s language of choice.  
  3. Call **`QueryLocalState`** (and related) methods to fetch live ledger state.  
  4. Receive strongly‑typed responses over Protobuf (optionally JSON‑encoded).  
* **Outcome:** The application accesses live node data directly, without Haskell dependencies or side‑car services (Ogmios, db-sync, etc).

### 3.2. Submitting Transactions (Any Backend Service)

* **Scenario:** A backend service constructs and signs Cardano transactions, then needs to broadcast them to the network.  
* **Actions:**  
  1. Use the generated RPC client stub to invoke **`SubmitTransaction`**, passing the signed transaction bytes.  
  2. If validation fails, handle the RPC error that returns the node’s precise rejection reason.  
  3. On success, the node accepts the transaction into its mempool and propagates it to peers.  
* **Outcome:** Transactions are submitted programmatically with immediate, actionable feedback from the node.

### 3.3. Managing RPC Endpoint (TLS and Runtime Settings)

* **Scenario:** An devOps engineer for a dapp must rotate TLS certificate, or adjust RPC configuration, without restarting the node.  
* **Actions:**  
  1. Place updated certificate/key (or new config) in the path monitored by **`cardano‑node`**.  
  2. The RPC server detects the change, reloads its configuration, and continues accepting connections with the new settings—ideally without dropping existing streams.  
* **Outcome:** RPC endpoints are maintained securely and with minimal downtime, ensuring continuous service for connected clients.

## 4\. High-Level Requirements

### 4.1. Functional Requirements (FR)

* **FR1: Protocol Method Coverage:** Implement RPC service methods mapping one-to-one with *all* available queries and actions within the `LocalStateQuery` and `LocalTxSubmission` mini-protocols. ^NOTE: A strict one-to-one mapping for *all* possible queries may become overly complex or incomplete if the underlying protocols evolve. Consider focusing on stable/critical queries first or providing an extensible design.  
    
* **FR2: Service Definitions & Client Assets:** Define clear, well-documented `.proto` files for the RPC services. Provide generated stubs or clear generation instructions for RPC supported languages (e.g. Go, Rust, Python, Node..)  
    
* **FR3: JSON/HTTP Interface:** Implement a mechanism to expose the RPC services via a JSON-over-HTTP interface.  
    
* **FR4: Conditional Activation:** The RPC server must only be activated if the `--enable-rpc` flag (or equivalent configuration) is provided. When disabled, it must incur close-to-zero performance overhead (watch for memory and cpu).  
    
* **FR5: Accurate Error Propagation:** For transaction submissions (`SubmitTransaction`), RPC errors returned to the client must include the precise validation error message provided by the underlying node logic.  
    
* **FR6: Dynamic Configuration Reload:** Implement support for reloading specific RPC server settings (initially TLS certificates and port) via a defined mechanism (e.g., `SIGHUP`, admin command) without a full node restart. Support for IPV4 and IPV6. Set RPC port with a flag at node startup (e.g. \--rpc-port)  
    
* **FR7: Monitoring Endpoint:** Provide a `/metrics` endpoint serving Prometheus-compatible metrics, including RPC request counts, error rates, request latencies, and active connection counts.

### 4.2. Non-Functional Requirements (NFR)

* **NFR1: Performance & Scalability:** The RPC server must handle a reasonable number of concurrent requests efficiently without significantly degrading overall `cardano-node` performance. Establish baseline performance benchmarks (we already have this data). Streaming responses for large data queries if applicable.  
    
* **NFR2: Reliability & Error Handling:** The server must handle network issues, malformed requests, and unexpected node states gracefully. Log errors clearly and utilize appropriate RPC status codes.  
    
* **NFR3: CIP must define Backward Compatibility & Versioning:** Future changes to `.proto` definitions must follow best practices (e.g., additive changes, reserved fields) to maintain backward compatibility. Implement a clear API versioning strategy.  
    
* **NFR4: Observability & Logging:** Integrate RPC server logs seamlessly with existing `cardano-node` logging. Provide sufficient metrics (see FR8) for operational monitoring.  
    
* **NFR5: Dynamic Reload Reliability:** Configuration reload events should minimize disruption to the service, ideally without dropping active connections unless absolutely necessary (e.g., port change). (Priority: Medium)

## 5\. Metrics for Success

* **Adoption:**  
    
  * Number of distinct projects/teams integrating the RPC API.  
  * Downloads/usage of client libraries/stubs.


* **Performance:**  
    
  * Average and p95 latency for key `LocalStateQuery` methods.  
  * Average latency for `SubmitTransaction` requests (RPC request to node mempool acceptance/rejection).  
  * Measured CPU and memory overhead added to `cardano-node` when the RPC server is active under load.


* **Stability & Reliability:**  
    
  * Uptime of the RPC service.  
  * Rate of RPC server errors (e.g., 5xx status codes) under normal load below a defined threshold.  
  * Successful dynamic configuration reloads reported by operators.


* **Community Feedback:** Qualitative feedback from developers and operators regarding ease of use, documentation quality, and feature completeness.

## 6\. Dependencies and Risks

| Dependency/Risk | Description | Mitigation Strategy |
| :---- | :---- | :---- |
| Ouroboros Mini-Protocols | Reliance on the stability and documented behavior of underlying protocols. | Close coordination with Consensus/Ledger teams; thorough integration testing. |
| TLS/mTLS Certificate Management | Operator error in managing certificates can lead to connectivity issues. | Provide clear documentation, examples, and robust error reporting for TLS setup. |
| Performance Impact | High RPC load could potentially impact node's primary functions. | Make RPC optional (FR4); implement efficient request handling; conduct load testing. |
| `.proto` Versioning | Breaking changes in API definitions can disrupt client applications. | Adhere strictly to backward compatibility rules (NFR3); clear versioning & comms plan. |
| Dynamic Reload Complexity | Implementing reliable hot-reloading without disrupting the node is complex. | Careful design; implement robust triggers/fallback logic; thorough testing of reload scenarios. |

## 7\. Roadmap

### Phase 1: Prototype & Design Finalization

* **Deliverables:**  
  * Draft `.proto` definitions for `LocalStateQuery` and `LocalTxSubmission`.  
  * Working Proof-of-Concept (PoC) implementation integrated with `cardano-node`.  
  * PoC client in at least one target language (e.g., Go or Rust) demonstrating query and submission.  
  * Detailed design for dynamic configuration reload mechanism.  
  * Initial benchmarking setup.

### Phase 2: Core Implementation & Security

* **Deliverables:**  
  * Production-ready RPC server implementation within `cardano-node`.  
  * Full support for TLS, mTLS, self-signed certificates, and insecure mode (FR5).  
  * Command-line flags/configuration for all RPC settings.  
  * Implementation of the chosen dynamic reload mechanism (FR7).  
  * Comprehensive automated tests (unit, integration, basic concurrency).

### Phase 3: JSON Interface, Client Support & Documentation

* **Deliverables:**  
  * Implementation of JSON/HTTP interface using RPC-Gateway or similar (FR3).  
  * Finalized `.proto` files published.  
  * Instructions/scripts for generating client stubs in Go, Rust, Python.  
  * Example client applications for each supported language.  
  * Detailed documentation for setup, configuration, security, dynamic reload, and API usage.

### Phase 4: Monitoring, Optimization & Maintenance

* **Deliverables:**  
  * `/metrics` endpoint implementation (FR8).  
  * Performance analysis and optimization based on benchmarking results (NFR1).  
  * Formalized API versioning strategy enacted (NFR3).  
  * Ongoing maintenance, bug fixes, and dependency updates.  
  * *Investigation* into exposing additional mini-protocols based on community demand.

## 8\. Definitions and Glossary

* **RPC:** An *inter‑process communication* paradigm that lets a program invoke a function (a “procedure”) that actually runs on another address space, typically a different machine on a network, *as if* it were a local call.  
* **gRPC:** A high-performance, open-source universal RPC framework.  
* **Protobuf (Protocol Buffers):** Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data.  
* **Mini-Protocols (Ouroboros):** Application-level protocols used for communication between Cardano nodes or between a node and a client (e.g., `cardano-cli`).  
* **LocalStateQuery:** Node-to-client mini-protocol for querying the node's current ledger state.  
* **LocalTxSubmission:** Node-to-client mini-protocol for submitting signed transactions to the node's mempool.  
* **gRPC-Gateway:** A gRPC ecosystem project that translates a RESTful JSON API into gRPC calls.  
* **Transcoding:** The process of converting data from one format or encoding to another (in this context, usually JSON/HTTP to gRPC/Protobuf).  
* **TLS (Transport Layer Security):** Cryptographic protocol designed to provide communications security over a computer network.  
* **mTLS (Mutual TLS):** TLS where both the client and server authenticate each other using certificates.  
* **Dynamic Reload:** Updating configuration parameters (e.g., TLS certificates) of a running service without requiring a restart.

## 9\. Appendices and References

* [gRPC Documentation](https://grpc.io/docs/)  
* [grapesy](https://github.com/well-typed/grapesy)  
* [Bitcoin rpc](https://developer.bitcoin.org/reference/rpc/)  
* [Ethereum rpc](https://ethereum.org/en/developers/docs/apis/json-rpc/)  
* [Protocol Buffers Documentation](https://protobuf.dev/overview/)

[^1]:  Follows IOG provided template  https://input-output.atlassian.net/wiki/spaces/PROD/pages/4162224214/Product+PRD+Template