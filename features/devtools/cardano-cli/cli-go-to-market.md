## Cardano-CLI Go-To-Market Plan

### Executive Summary

The goal of this GTM plan is to effectively communicate the new features and improvements in Cardano-CLI to its diverse user base. The marketing approach will focus on developer outreach, ecosystem engagement, and clear documentation updates. Success will be measured by adoption rates, developer feedback, and community engagement metrics.

### The 4 Ps

* **Product:** A powerful command-line tool enabling seamless interaction with the Cardano blockchain.  
* **Price:** Open-source and free to use.  
* **Place:** Accessible via GitHub, official Cardano developer portals, and bundled with every node release.  
* **Promotion:** Developer blogs, release notes, ecosystem AMAs, community engagement, and documentation updates.

### Vision

To be the most powerful and up-to-date tool for blockchain operations, setting the standard for transparency, control, and accessibility in decentralized infrastructure.

### Strategy

Prioritize architectural flexibility to seamlessly support future protocol upgrades and evolving Cardano-node capabilities. Focus on improving UX/UI, simplifying blockchain interactions, and ensuring comprehensive documentation.

### Product positioning

#### Problems Cardano-CLI Solves

* **Direct interaction with cardano-node:** Cardano-CLI provides a trustless way to interact with the Cardano blockchain without relying on third-party services.  
* **Complex transaction creation:** The CLI enables users to construct, sign, and submit complex transactions, including: certificates, governance actions, votes, deployment and interaction with plutus scripts, and stake pool management.  
* **Limited automation options:** Enables developers and system administrators to script and automate blockchain interactions, improving efficiency.  
* **Governance participation barriers:** Supports creating, submitting, and voting on all types of governance actions, making on-chain governance accessible to key stakeholders.  
* **Testing and debugging challenges:** Provides deep blockchain querying capabilities, aiding test engineers and developers in debugging and validating blockchain interactions.

### Competitive Advantage

* Direct interaction with Cardano-Node, ensuring decentralization.  
* Enables comprehensive end-to-end testing by allowing precise control over blockchain interactions, transaction execution, and state queries.  
* Full support for governance, stake pool operations, and advanced transaction management.  
* Continuous improvements in usability, automation, and integration with ecosystem tools.  
* Robust integration with the node thanks to the use of typed protocols provided through the Cardano-API.  
* Full stake pool operations are ONLY supported by the CLI.   
* Full support of governance features.

### Target Audience

* **Stake Pool Operators**: Need CLI for managing pools efficiently.  
* **Developers**: Require direct blockchain access for application development.  
* **System administrators**: Maintain blockchain infrastructure and nodes.  
* **Constitutional committee members & Delegated representatives**: Use CLI for governance actions and voting.  
* **Test Engineers**: Validate network behavior and blockchain interactions.  
* **ADA Holders**: Individuals who wish to engage in governance, voting, staking, and direct blockchain interactions.

### Channel Strategy

* **Developer engagement**: Blog posts, AMAs, DevPulse podcast, workgroups.  
* **Community engagement**: Direct feedback loops via discord workgroups and github issues.  
* **Technical documentation**: Comprehensive end-user documentation in developers.cardano.org  
* **Workshops & webinars**: Demonstrations of new features.  
* **Social media**: Twitter(x) and Discord for announcements and discussions.

### Product Launch Plan

* **Core Message:** Cardano-CLI interacts directly with a local Cardano node, fostering decentralization and giving users full control over their blockchain interactions. It is suitable for both human users and automation via scripting, with no limitations on functionality. 

* Go-to-Market Strategy:  
  * Publish a **workshop/webinar demo video** on X (Twitter) and **Intersect workgroups** for every new feature release.  
  * Ensure every new feature is **accompanied by relevant documentation updates** on [developers.cardano.org](https://developers.cardano.org).  
  * Engage with the community through **AMAs, ecosystem discussions, and developer forums** to explain and demonstrate new features and receive feedback.  
  * **Promote feature releases** through IO and Intersect official social media channels.

### Customer Onboarding

* Quick Start Guides: Provide easy-to-follow documentation for key features within cardano-cli’s repository.  
* **Official Documentation:** Ensure comprehensive and up-to-date guidance, step-by-step guides for common workflows.

### Metrics and KPIs

Suggested metrics are Cycle Time, WIP, Throughput, and Work Item Age. Tracking these, the team can transform an unpredictable process into one that reliably forecasts delivery.

These metrics provide clear insights into where delays or bottlenecks occur, enabling us to make informed, consistent predictions about when customer value will be delivered.

* **Cycle Time:** Measures the elapsed time from when a work item starts until it is completed. This helps assess responsiveness to customer feedback. Time-to-market for a given feature.  
* **Work in Progress (WIP):** Tracks the number of active development tasks to prevent excessive work accumulation and aging of unresolved issues.  
* **Throughput:** Measures the number of work items completed per unit of time, providing insights into development speed and efficiency.  
* **Work Item Age:** Tracks the time elapsed since a work item started, helping monitor delays and identify bottlenecks in the development process.  
* **Release Frequency:** how often cardano-cli is released.

### Risk Management

* **Risk: Being left out of the budget**  
  * Mitigation**: ?**  
* **Risk: Low adoption of new features**  
  * Mitigation: Early user feedback, thorough documentation, and developer outreach.  
* **Risk: Backward compatibility issues/ breaking changes**  
  * Mitigation: Clear deprecation roadmap and migration guides.  
* **Risk: Unexpected bugs or usability challenges**  
  * Mitigation: Expand test coverage, beta testing, and rapid response to user issues.  
* **Risk:  Tech debt accumulation:**  
  * Mitigation: Dedicate 5-10% of time to payout tech debt.  

### Cardano-CLI Roadmap

1. **UI/UX improvements**  
   * **Goal:** Clean up and streamline the user interface.  
   * **Key items:**  
     * Reorganize the top level command structure  
     * Improve help texts for better clarity   
     * Improve error messages during transaction build and submit  
     * Remove support for eras prior to Conway.  
     * Json schemas for CLI outputs.
2. **Feature expansion**   
   * **Goal:** Enhance the cardano-cli feature set with new capabilities.  
   * **Key items:**  
     * Feasibility analyzis for functionality to **balance hydra-generated incomplete transactions.**  
     * Add support for **CIP129** and DREP IDs.  
     * Develop a command to **decode CLI certificates** into a human-readable format  
     * Investigate the feasibility of **validating transactions** against the current ledger state **without submission.**  
     * How can we improve UX in build? it has grown too big. Perhaps break it down in pieces? Partially construct bits of tx-body? A command for simple transactions? Better documentation? Interactive?   
     * **Nested transactions:** Prepare for potential integration of Nested transactions (CIP118). This will be implemented by ledger, probably in a feature branch? This will need extensive changes to the CDDL.   
     * **Hardware wallet support.**   
     * **Ouroboros Peras preparation:** Investigate and document any potential requirements  
3. **Documentation**  
   * **Goal:** Ensure clear, comprehensive, and up-to-date documentation to improve user onboarding, enhance developer experience, and facilitate easier adoption of new features.  
   * Key items:  
     * Update README with link to developers.cardano.org  
     * Comprehensive command reference for new features.  
     * Ensure 100% coverage of features.   
     * Troubleshooting & debugging documentation  
4. **Maintenance**  
   * **Goal:** Ensure long-term sustainability, efficiency, and developer experience improvements and enhancing overall developer quality of life.  
   * Key items:  
     * Upgrade to use new API 
     * Analyze increasing tests coverage with cardano-testnet on CLI repo.
