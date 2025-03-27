# Nested Transactions PRD (DRAFT)

**Dependency-Driven Transaction Bundling for Enhanced Transaction Balancing and
Composability**

**Document owner:** Samuel Leathers

**Responsible:** Ledger Team, led by Alexey Kuleshevich

**Accountable:** Ricky Rand (IOHK)

**Consulted:** Samuel Leathers, Carlos Lopezdelara, Core Development Teams

**Informed:** Cardano Community, Resource Providers (Babel Fee Providers,
Min-UTxO Providers)

**Summary:** This document outlines the product requirements for implementing
transaction batching and dependent execution on the Cardano blockchain. The
primary objective is to enable intent-based transaction balancing and
composability, facilitating use cases like Babel fees and Min-UTxO provision.

## Executive Summary

* **Objective:** To introduce a mechanism for aggregating multiple
  "sub-transactions" into a single on-chain submission, enabling the definition
  of dependencies and "intents" (specifically, fee payment and Min-UTxO
  provision) between these transactions. This will primarily support Babel fee
  implementations and Min-UTxO provisioning, where users can specify intents on
  how the transaction can be balanced, and facilitate the concurrent balancing
  of those intents into a single transaction. It also enhances the ability to
  compose transactions.
* **Target Audience / Stakeholders:**
    * Cardano dApp developers implementing Babel fees, Min-UTxO provisioning,
      or complex transactions with external balancing requirements.
    * Cardano node operators.
    * Resource providers (Babel fee providers, Min-UTxO providers).
    * Decentralized and Centralized Exchanges and high-volume transaction processors.
    * End-users seeking transaction balancing and composability.
* **Vision:** To significantly enhance Cardano's transaction flexibility and
  enable complex, intent-driven transaction balancing and composability,
  contributing to a more user-friendly and versatile blockchain platform.

## Goals and Non-Goals

* **Goals:**
    * Enable the definition and execution of "intents" related to fee payment
      and Min-UTxO provisioning within batch transactions.
    * Facilitate the implementation of Babel fees, allowing users to pay fees
      in non-ADA currencies.
    * Enable Min-UTxO provisioning, allowing users to specify intents on how
      Min-UTxO requirements can be balanced.
    * Enable the definition and enforcement of transaction dependencies within
      a batch, ensuring the correct balancing of subtransactions.
    * Enhance smart contract capabilities with complex transaction balancing
      and composability.
* **Non-Goals:** (outside of scope)
    * Direct reduction of base transaction fees (the focus is on transaction
      balancing, not reduction).
    * Increase of transaction throughput (TPS).
    * Specific Layer 2 implementations (e.g., detailed rollup specifications).
    * Detailed cryptographic protocols for advanced privacy features within
      batches (initially).
    * Complete, finalized user interface designs for wallets or dApps using
      these features.
    * Specific Light client implementations.
    * Development of off-chain marketplace for Babel fees.

## User Journeys

* **Persona 1: dApp Developer (Alice) implementing Babel fees and Min-UTxO
  provisioning:**
    * Alice wants to enable users of her dApp to pay transaction fees in
      Bitcoin and to easily create UTxOs that meet the Min-UTxO requirements.
    * Alice uses the batching and dependency feature to define "intents" within
      the transaction, specifying intents on how the transaction can be
      balanced regarding fee payment and Min-UTxO provisioning.
    * Alice defines dependencies to ensure that the transaction is balanced
      only after a Babel fee provider covers the ADA transaction fee and a
      Min-UTxO provider fulfills the Min-UTxO requirement, all happening
      concurrently within the main transaction.

* **Persona 2: DEX Developer (Bob) implementing trade aggregation:**
    * Bob wants to build a decentralized exchange (DEX) that can
        efficiently process multiple trades in a single transaction.
    * Bob uses the batching and intent feature to allow users to
        submit their trades as "intents" (sub-transactions).
    * Bob's DEX assembles multiple user trades into a single top-level
        transaction, balancing the inputs and outputs, while keeping
        some profit for himself.
    * Bob submits this transaction on-chain, thus ensuring the trades are
        executed atomically.
    * This implementation allows for more efficient trade execution,
        reducing fees and improving the user experience compared to
        processing each trade individually.

## High-Level Requirements

* **Functional Requirements:**
    * **Mempool Acceptance:**
        * The Cardano node's mempool shall accept transactions containing
          sub-transactions.
    * **Transaction Balancing:**
        * Individual sub-transactions shall not be required to be balanced.
        * The top-level transaction shall be required to be balanced.
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

## Metrics for Success

* **Impact Metrics:**
    * Adoption rate of Babel fee and Min-UTxO balancing implementations by dApp
      developers.
    * Number of Babel fee and Min-UTxO balancing transactions processed.
    * Adoption rate of batching by dApp developers.
    * Reduction in failed transaction rates.
    * Latency of batched transactions.

## Dependencies and Risks

* **Dependencies:**
    * Updates to the Cardano ledger protocol, specifically related to intent
      definition and transaction balancing.
    * Plutus platform enhancements.
    * Cardano node software updates.
    * CIP-0118 implementation.
* **Risks:**
    * Potential security vulnerabilities in the transaction balancing
      mechanism.
    * Complexity of implementation and testing, especially with resource
      providers.
    * Potential for breaking changes to existing dApps.
    * Unexpected performance bottlenecks.

## Roadmap

* Phase 1: Research and design, CIP-0118 refinement and approval.
* Phase 2: Core protocol implementation and Plutus integration, with specific
  attention to Babel fee and Min-UTxO support.
* Phase 3: Testing and auditing, including testing with resource providers.
* Phase 4: Mainnet deployment.
* Phase 5: Developer tooling, documentation, and resource provider onboarding.
