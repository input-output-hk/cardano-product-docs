# Nested Transactions User Guide

Owned by Samuel Leathers

Last updated: 2025-03-26

Document owner: Samuel Leathers

Initiative: Dijkstra Era Nested Transactions

Responsible: Ledger Team

Accountable: Ricky Rand (IOHK)

Consulted: Carlos Lopezdelara

Informed: Cardano Community, Resource Providers (Babel Fee Providers, Min-UTxO
Providers)

## Summary

This guide provides step-by-step instructions for developers and resource
providers on how to create and interact with Nested Transactions on the Cardano
blockchain. It covers the process of building transactions with intents,
submitting them, and fulfilling those intents by resource providers.

## Overview

This guide aims to enable users to effectively use the Nested Transactions
feature for complex transaction scenarios like swaps to cover fees and Min-UTxO
provisioning. By following this guide, users will learn how to create and
manage transactions with sub-transactions and intents. It assumes a basic
understanding of Cardano transaction construction and Plutus scripts.

## Prerequisites

* **Purpose:** Ensure users have the necessary setup and knowledge before
  proceeding.
* **Environment Setup:**
    * Running Cardano node (version supporting Nested Transactions).
    * Installed Cardano CLI tools.
    * Access to a development environment for Plutus scripts (if applicable).
    * Access to a mempool monitoring tool.
* **Knowledge Requirements:**
    * Familiarity with Cardano transaction construction and balancing.
    * Basic understanding of Plutus scripts (for Plutus-enabled intents).
    * Knowledge of UTxO model.
* **Tools Needed:**
    * Cardano CLI.
    * Cardano SDK (if using SDK for transaction construction).
    * Mempool monitoring tools.

## Process Overview

1.  **Sub-Transaction Construction:** Building individual sub-transactions with
intents.
2.  **Intent Encoding:** Encoding intents for swaps using the `--intents` flag
with the "swaps" key.
3.  **Signing:** Signing sub-transactions with required witnesses.
4.  **Top-Level Transaction Construction:** Building the top-level transaction
that aggregates the sub-transactions.
5.  **Submission:** Submitting the top-level transaction to the mempool.
6.  **Intent Fulfillment:** Resource providers monitoring and fulfilling
intents by submitting balancing sub-transactions.
7.  **Validation and Inclusion:** Cardano node validating and including the
balanced top-level transaction in a block.

## Step-by-Step Instructions

### 1. User Actions (Transaction Construction - Developer)

#### 1.1 Building Sub-Transactions with Intents

* **Objective:** Construct individual sub-transactions with intents for swaps.
* **Steps:**
    1.  Create sub-transactions using the Cardano CLI or SDK.
    2.  Encode intents using the `--intents` flag, specifying the "swaps" key,
    asset, amount, and minimum Lovelace amount.
* **Command Template (Example):**

```bash
cardano-cli transaction build --tx-in <input-utxo> --tx-out \
    <output-address>+<amount> \
    --intents '{ "swaps": [{ "asset": "HOSKY", "amount": 1000000, "min_lovelace": 2000000 }] }' \
    --out-file alice_sub_tx.draft
```

* **Example Command:**

```bash
cardano-cli transaction build --tx-in 1234abcd...#0 --tx-out \
    addr1... +5000000 \
    --intents '{ "swaps": [{ "asset": "HOSKY", "amount": 1000000, "min_lovelace": 2000000 }] }' \
    --out-file alice_sub_tx.draft
```

* **Parameters Explained:**
    * `--tx-in`: Input UTxO for the sub-transaction.
    * `--tx-out`: Output address and amount for the sub-transaction.
    * `--intents`: JSON object containing the intents, including the swap
      intent using the "swaps" key.
    * `"swaps"`: Key for the array of swap intents.
    * `asset`: The native asset to be swapped (e.g., "HOSKY").
    * `amount`: The quantity of the native asset to be swapped (e.g.,
      1000000).
    * `min_lovelace`: The minimum Lovelace amount required for the swap
      (e.g., 2000000).
    * `--out-file`: Output file for the draft sub-transaction.

#### 1.2 Signing Sub-Transactions

* **Objective:** Sign sub-transactions with the necessary witnesses.
* **Steps:**
    1.  Determine the necessary witnesses for the input UTxOs of each
    sub-transaction.
    2.  Sign the sub-transactions using the Cardano CLI or SDK, providing the
    required signing keys.
* **Command Template (Example):**

```bash
cardano-cli transaction sign --tx-body-file alice_sub_tx.draft \
    --signing-key-file alice.skey --out-file alice_sub_tx.signed
```

* **Example Command:**

```bash
cardano-cli transaction sign --tx-body-file alice_sub_tx.draft \
    --signing-key-file alice.skey --out-file alice_sub_tx.signed
```

* **Parameters Explained:**
    * `--tx-body-file`: Input draft sub-transaction file.
    * `--signing-key-file`: Signing key file for the witness.
    * `--out-file`: Output signed sub-transaction file.

### 2. Resource Provider Actions (Intent Fulfillment)

#### 2.1 Monitoring and Fulfilling Intents

* **Objective:** Monitor the mempool for transactions with intents and fulfill
  them by balancing and submitting sub-transactions.
* **Steps:**
    1.  Monitor the mempool for transactions containing sub-transactions with
    specific swap intents.
    2.  Analyze the swap intents to determine the required actions (e.g.,
    providing ADA for fees, Min-UTxO).
    3.  Construct and submit sub-transactions to balance the top-level
    transaction, ensuring the minimum Lovelace requirement is met.
    4.  Verify that the top-level transaction is balanced after submitting the
    balancing sub-transactions.
* **Command Template (Example):**

```bash
cardano-cli transaction build --tx-in <resource-provider-utxo> \
    --sub-tx ./alice_sub_tx.signed --sub-tx ./bob_sub_tx.signed --fee \
    <fee-amount> --change-address <change-address> --tx-out \
    <resource-provider-address>+<cut-amount> --out-file balancing_tx.draft
```

* **Example Command:**

```bash
cardano-cli transaction build --tx-in 5678efgh...#0 --sub-tx \
    ./alice_sub_tx.signed --sub-tx ./bob_sub_tx.signed --fee 200000 \
    --change-address addr1... --tx-out addr1...+100000 --out-file \
    balancing_tx.draft
```

* **Parameters Explained:**
    * `--tx-in`: Input UTxO from the resource provider.
    * `--sub-tx`: Specifies the signed sub-transaction, named after the actor
      (e.g., alice_sub_tx). Can be repeated.
    * `--fee`: Fee amount for the balancing transaction.
    * `--change-address`: Address for change from the swap.
    * `--tx-out`: Output address and cut amount for the resource provider
    * `--out-file`: Output file for the balancing sub-transaction.draft

### Definitions

| Term                | Description                                                                                             |
| ------------------- | ------------------------------------------------------------------------------------------------------- |
| Sub-transaction     | A component transaction within a larger, top-level transaction.                                          |
| Top-level transaction | A transaction that aggregates one or more sub-transactions.                                             |
| Intent              | A specification within a sub-transaction outlining how it should be balanced (e.g., fee payment, Min-UTxO). |
| Swaps               | A type of intent specified within the `--intents` flag, using the "swaps" key, that requests a given amount of a native asset to be exchanged for a minimum Lovelace amount. |
| Min-UTxO            | The minimum amount of ADA required for a UTxO.                                                         |
| Resource Provider   | An entity that provides resources (e.g., ADA for fees, Min-UTxO) to balance transactions.             |
| Witness             | A signature or other cryptographic proof required to authorize spending from a UTxO.          |
| Collateral          | A designated UTxO provided to cover potential fees in case a transaction fails validation.         |
| Change Address      | An address used to receive any remaining funds from a transaction that are not explicitly spent as outputs. |
| Cut Amount          | The portion of ADA taken by the resource provider as a fee for their services. |
