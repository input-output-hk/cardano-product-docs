# Nested Transactions Feature Requirements

Owned by Samuel Leathers

Last updated: 2025-03-26

Accountable: Ricky Rand (IOHK)

Consulted: Carlos Lopezdelara

Target release: Dijkstra Era

Epic: [Insert Jira link]

Informed: Cardano Community, Resource Providers (Babel Fee Providers, Min-UTxO Providers)

Initiative: [Insert Jira Link]

Document status: Initial Draft

Document owner: Samuel Leathers

Designer: Polina Vinogradova

Engineering Manager: Alexey Kuleshevich

QA: Ledger Team, SDET Team

Responsible: Ledger Team

## 1. Summary

* **Purpose:** Provide an overview of the Nested Transactions feature and its
  importance.
* **Feature:** Nested Transactions enable the aggregation of multiple
  "sub-transactions" into a single top-level transaction, facilitating
  intent-based transaction balancing and composability.
* **Context:** This feature addresses the need for more efficient and flexible
  transaction handling on the Cardano blockchain, particularly for use cases
  like Babel fees, Min-UTxO provisioning, and DEX trade aggregation.
* **Mechanism:** The feature involves modifications to the Cardano node's
  mempool and transaction validation logic, along with Plutus integration to
  allow smart contracts to interact with nested transactions.
* **Goal:** To enhance Cardano's transaction flexibility, lower barriers to
  entry for dApp developers, and improve the user experience by enabling
  complex transaction scenarios.

## 2. Assumptions

* CIP-0118 will be successfully approved and implemented.
* Resource providers (Babel fee providers, Min-UTxO providers) will adopt the
  new mechanisms.
* The Cardano community will find the feature useful and integrate it into dApp
  development.
* Plutus integration will be successfully implemented and function as intended.

## 3. Constraints

* Implementation must maintain compatibility with existing Cardano
  infrastructure.
* Development must adhere to security best practices to prevent
  vulnerabilities.
* Implementation must be scalable to handle increasing transaction volumes.
* Time and resource constraints may impact the timeline for development and
  deployment.
* Potential complexity of testing and auditing, especially with resource
  provider integration.

## 4. User Flows

* **Purpose:** Detail how different types of users will interact with the
  Nested Transactions feature.
* **User Definition:**
    * Cardano dApp Developers (Alice & Bob)
    * Resource Providers (Babel Fee Providers, Min-UTxO Providers)
* **User Journeys:**
    * **Alice (Babel Fees & Min-UTxO):**
        * Alice defines "intents" within a transaction to specify how it should
          be balanced regarding fee payment (Babel fees) and Min-UTxO
          provisioning.
        * Alice ensures the transaction is balanced only after resource
          providers fulfill these intents, all within a single transaction.
    * **Bob (DEX Trade Aggregation):**
        * Bob creates a DEX that aggregates user trades into a single top-level
          transaction.
        * Bob allows users to submit trades as "intents" (sub-transactions).
        * Bob's DEX balances the inputs and outputs, while taking a profit, and
          submits the transaction on-chain for atomic execution.
    * **Resource Providers:**
        * Resource providers monitor the mempool for transactions with intents
          they can fulfill.
        * Resource providers submit sub-transactions to balance the top-level
          transaction, fulfilling the specified intents.
* **Data Flow:**
    * User intents are encoded within sub-transactions.
    * Sub-transactions are submitted to the mempool as part of a top-level
      transaction.
    * Resource providers retrieve intent information from the mempool.
    * Resource providers submit sub-transactions to fulfill intents.
    * The Cardano node validates and processes the top-level transaction and
      its sub-transactions.
    * The balanced top-level transaction is included in a block.

## 5. Components

* **Purpose:** Break down the elements required to build the feature.
* **Node Software:**
    * Modified mempool logic to accept and validate transactions with
      sub-transactions.
    * Updated transaction validation logic to handle intent-based balancing.
    * Plutus integration to allow Plutus scripts to access the full top-level
      transaction context.
* **Network Components:**
    * Cardano node operators: Responsible for processing and validating nested
      transactions.
    * Resource providers: Responsible for monitoring the mempool and providing
      sub-transactions to balance transactions.
    * Visualization: A diagram from the PRD showing the interaction between the
      mempool, the node and resource providers would be beneficial here.
* **Recommendation:** Supplement this section with a diagram from the Product
  Requirements Document (PRD) that illustrates the stack components.

## 6. Phases

* **Purpose:** Feature implementation phases.
* **Phase 1: CIP-0118 Refinement and Approval:**
    * Finalize and gain approval for CIP-0118, which defines the core
      mechanisms for intent-based transaction balancing.
* **Phase 2: Core Protocol Implementation and Plutus Integration:**
    * Implement the necessary changes to the Cardano ledger protocol and node
      software.
    * Integrate Plutus to enable smart contracts to work with nested
      transactions.
* **Phase 3: Testing and Auditing:**
    * Conduct thorough testing and security audits, including testing with
      resource providers.
* **Phase 4: Mainnet Deployment:**
    * Deploy the feature to the Cardano mainnet.
* **Phase 5: Developer Tooling and Resource Provider Onboarding:**
    * Develop developer tools and documentation.
    * Onboard resource providers to the ecosystem.
* **Recommendation:** A visual representation of the phases would be beneficial
  here.

## 7. Requirements

* **Purpose:** Functional and non-functional requirements.
* **Functional Requirements:**
    * **Mempool Acceptance:**
        * The Cardano node's mempool shall accept transactions containing
          sub-transactions.
    * **Transaction Balancing:**
        * Individual sub-transactions shall not be required to be balanced.
        * The top-level transaction shall be required to balance the aggregate
          of all sub-transactions.
    * **Min-UTxO Handling:**
        * If a sub-transaction includes outputs that do not meet the Min-UTxO
          requirement, then another sub-transaction within the same top-level
          transaction must spend these outputs.
    * **Plutus Script Execution:**
        * Sub-transaction builders shall be able to require the execution of
          Plutus scripts.
        * The full top-level transaction, including all sub-transactions, shall
          be available within the Plutus script context.
    * **Collateral Responsibility:**
        * The top-level transaction builder shall be responsible for providing
          collateral.
    * **Plutus Script Compatibility:**
        * Existing Plutus scripts shall remain unaffected.
* **Non-Functional Requirements:**
    * Security and integrity of batch transactions, including the handling of
      transaction balancing.
    * Usability for developers and resource providers.
    * Scalability to handle increasing transaction volumes.
    * Compatibility with existing Cardano infrastructure.
* **Visual Aids:** Sequence and flow diagrams for transaction processing are
  recommended.

## 8. Definitions

* **Purpose:** Glossary of terms.

| Term                | Description |
| ------------------- | ----------- |
| Sub-transaction     | A component transaction within a larger, top-level transaction. |
| Top-level transaction | A transaction that aggregates one or more sub-transactions. |
| Intent              | A specification within a sub-transaction outlining how it should be balanced (e.g., fee payment, Min-UTxO provision). |
| Babel fees           | A mechanism allowing users to pay transaction fees in non-ADA currencies. |
| Min-UTxO            | The minimum amount of ADA required for a UTxO. |
| Transaction Balancing | The process of ensuring that the inputs and outputs of a transaction are equal. |
| Resource Provider   | An entity that provides resources (e.g., ADA for fees, Min-UTxO) to balance transactions. |
