# Cardano DevTools Suite

## Mission

Our mission, derived from [2025 Vision & Roadmap for Cardano](https://product.cardano.intersectmbo.org/vision-roadmap-2025/#developeruser-experience), is to empower developers, stake pool operators, governance actors, ada-holders and innovators with robust, user-friendly tools that seamlessly interact **with a local cardano-node.**

## Vision

Equip users with the essential tools they need—whether by enhancing existing solutions or pioneering new ones—to interact with Cardano in a friendly, intuitive, and high-performance manner. Our vision is to lead in delivering capabilities that simplify interactions with cardano-node, streamline development workflows, and ensure broad accessibility across multiple programming environments, making Cardano the most developer-friendly blockchain.

 We aim to:

* Lower the barrier to entry for developers.  
* Provide robust, well-documented tools.  
* Enable seamless, real-world testing environments.  
* Support multi-language interoperability.

Guided by these principles, we will enhance each tool to help developers focus on innovation.

## Product Suite

The Cardano Developer Tools suite consists of four key components designed to address different aspects of blockchain development. These tools interact directly with a running Cardano node, ensuring a decentralized and trustless development experience.

* [**Cardano-CLI**](https://github.com/IntersectMBO/cardano-cli): A powerful command-line interface providing direct access to Cardano's blockchain functionality. It allows users to create and sign transactions, manage keys and addresses, create governance actions, create all types of certificates and retrieve blockchain data. Essential for stake pool operators, developers, and power users who need full control over blockchain interactions.  
* [**Cardano-Testnet**](https://github.com/IntersectMBO/cardano-node/tree/master/cardano-testnet): A standalone tool designed for easy creation of local test networks. It enables developers to test their applications in a controlled environment that mirrors the real Cardano blockchain, making it an essential resource for debugging, experimentation, and simulation of network conditions. This is essential for testing governance related functionality, since in real networks the control of governance is distributed.  
* [**Cardano-API**](https://github.com/IntersectMBO/cardano-api): A Haskell library that serves as the backbone for application development on Cardano. It provides structured, programmatic, robust, tightly integrated, and comprehensive access to blockchain features, enabling developers to integrate Cardano into their applications with high reliability and precision. The **Cardano-CLI** itself is powered by this API.  
  * **WASM library\***:  
* **Cardano-Node gRPC\***: An upcoming gRPC-based RPC server that provides a language-agnostic interface for interacting with the Cardano node. Most likely built as a wrapper around the existing node-to-client miniprotocols, it simplifies integration for developers using non-Haskell languages, making Cardano  more accessible to a broader developer audience.

Together, these tools provide a comprehensive and flexible ecosystem for building, testing, and deploying applications while maintaining the decentralization and trustless principles that underpin the blockchain.