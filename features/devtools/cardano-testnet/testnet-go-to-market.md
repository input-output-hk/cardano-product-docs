# cardano-testnet

## Cardano-Testnet Go-To-Market Plan

### Executive Summary

Cardano-Testnet is a robust and user-friendly solution designed to simplify the deployment and management of customizable Cardano test networks. It empowers developers, operators, governance groups, researchers, and educators by providing a secure and isolated environment that mirrors real-world blockchain conditions. With automation at its core, Cardano-Testnet eliminates the complexities associated with setting up test environments, enabling rapid iteration, thorough testing, and confident experimentation before mainnet deployment. This tool not only accelerates development cycles but also enhances the quality and reliability of blockchain applications, fostering innovation across the Cardano ecosystem.

### The 4 Ps

* **Product**: Cardano-Testnet is a fully customizable test network solution for Cardano. It automates the setup process, allowing users to quickly spin up both local and distributed testnets with a single command. Designed to replicate mainnet conditions in a controlled environment, it provides developers with the ability to test smart contracts, governance proposals, and protocol changes in a safe, isolated space.

* **Price**: Cardano-Testnet is offered as an open-source tool, making it freely available to all users.

* **Place**: The tool will be accessible through GitHub, official Cardano developer portals,  and bundled with every node release.

* **Promotion**:  
  The promotion strategy centers on active developer and community engagement. This includes targeted blog posts, detailed technical documentation on [developers.cardano.org](http://developers.cardano.org), interactive webinars and workshops, and announcements via social media channels like Twitter (X) and Discord. Additional outreach efforts will involve ecosystem collaborations and feedback loops to continuously refine and enhance the tool based on user needs.

### Vision

The easiest and most reliable way for developers, operators, governance groups, researchers and instructors to spin up and control real Cardano testnets, enabling innovation, testing, and education in a safe, fully customizable blockchain environment.

###  Product positioning

#### Problems Cardano-Testnet Solves

* **Complexity in setting Up Test environments:** Running a Cardano test network manually requires deep technical knowledge and extensive configuration. Cardano-Testnet automates this process, allowing users to spin up fully functional test networks with a single command.

* **Lack of controlled & repeatable testing conditions:** Public testnets may not always be suitable for controlled testing scenarios. For example, testing a Dapp behavior during a hardfork. Cardano-Testnet allows developers to configure custom environments, set specific network parameters, and reset the chain state when needed.

* **Difficulty in simulating real-world scenarios:** Testing smart contracts, governance proposals (and its consequences), or network upgrades requires an isolated and reproducible environment. The tool enables users to create and manipulate private test networks for **protocol research, governance experiments, and more.**  

* **Onboarding challenges for new developers, researchers, governance actors, and other users:** Getting started with blockchain development requires a **low-risk sandbox** where users can experiment without real-world consequences. Cardano-Testnet provides a beginner-friendly way to interact with the Cardano blockchain without requiring mainnet ADA.

* **Ensuring software compatibility before deployment:** DApp developers, stake pool operators, and governance groups need to **test transactions, contracts, and protocol changes** before deploying to mainnet. Cardano-Testnet ensures smooth transitions by mirroring real-world conditions while remaining completely isolated.

### Competitive Advantage

While other blockchain ecosystems offer similar testing tools, Cardano-Testnet will leap ahead by providing a significantly simpler and more streamlined setup, making it the most accessible and efficient testnet deployment solution available. Compared to other platforms:

* **Fully customizable test networks**  
  * **Cardano-Testnet:** Allows users to configure parameters such as slot times, epochs, network topology, and transaction fees to match specific testing needs.​  
  * **Ethereum:** Tools like Geth enable custom testnets, but require manual setup and configuration.   
  * **Solana:** The Solana Test Validator provides a local emulator, but customization options are more limited compared to Cardano-Testnet.   
  * **Polkadot:** Tools like Substrate and Zombienet allow for local testnets, but may involve more complex configurations. 

* **Rapid Testnet deployment**  
  * **Cardano-Testnet:** Enables launching a private testnet with a single command, significantly reducing setup time and complexity.​  
  * **Ethereum:** Setting up a local testnet involves multiple steps and tools, which can be time-consuming.  
  * **Solana:** On pair with the Solana Test Validator, which allows for quick local testnet setup.  
  * **Polkadot:** Deploying a local testnet requires using Substrate or similar frameworks, which can be complex for new users. 

* **Local and distributed testing**  
  * **Cardano-Testnet:** Supports both local test environments and multi-node distributed testnets, enabling robust application and infrastructure testing.​  
  * **Ethereum:** Setting up a multi-node testnet is possible, configuration is arguably more complex.  
  * **Solana:** Primarily supports single-node local validators; setting up a multi-node testnet locally is more complex.  
  * **Polkadot:** Supports multi-node testnets through Substrate, but setup can be intricate and resource-intensive.

Target Audience

Cardano-Testnet serves a diverse set of users who need a **controlled, customizable, and reproducible blockchain environment** for testing, development, governance, and education.

* **Developers & DApp Builders** – Test and debug applications in an isolated, cost-free environment before mainnet deployment.  
* **Smart Contract & DeFi Developers** – Validate Plutus and Aiken contracts. Connect local Dapps to the local testnet.   
* **Governance Groups & Constitutional Committees** – Simulate governance proposals, test voting mechanisms, train operators/orchestrators and validate protocol changes.   
* **Stake Pool Operators & Infrastructure Engineers** – Learning to configure and operate stake pools.  
* **Educators & Blockchain Instructors** – Use testnets as a hands-on teaching tool for blockchain development and governance.

### Channel Strategy

To maximize adoption and awareness of **Cardano-Testnet**, we will engage the community through multiple targeted channels, ensuring that key stakeholders—developers, operators, governance groups, researchers, and educators—are well-informed and equipped to leverage the tool effectively.

* **Developer engagement** – Blog posts, AMAs, and deep-dive technical content explaining how to set up and use Cardano-Testnet.  
* **Community engagement** – Discussions in **Intersect workgroups, Twitter Spaces, and Discord forums** to gather feedback and showcase use cases.  
* **Technical documentation & tutorials** – Comprehensive guides on [developers.cardano.org](https://developers.cardano.org) to walk users through testnet deployment and customization.  
* **Workshops & Webinars** – Hands-on demonstrations for developers, governance groups, and educators on best practices for testing in a controlled environment.  
* **Social media & ecosystem promotion** – Feature highlights and walkthroughs shared via Twitter, LinkedIn, and other key blockchain channels.  
* **Collaboration with infrastructure providers** – Work with key ecosystem players to ensure seamless integration into developer workflows.


### Product Launch Plan

**Core Messaging:**  
 • “The simplest way to spin up and control a fully customizable Cardano test network.”  
 • “Backed by IO’s long-standing experience using this tool to rigorously test cardano-node and cardano-cli features, now released to empower the community's own development and testing efforts.”

#### Launch Strategy:

**Pre-Launch**  
 • Gather feedback from early adopters (developers, SPOs, governance groups).  
 • Prepare detailed documentation and setup guides.  
 • Announce upcoming availability through developer forums and ecosystem groups.

**Feature Release Approach**  
 • Each major feature update will be announced with:  
   \- Workshop/Webinar & Demo Video (published on X/Twitter and Intersect workgroups).  
   \- Updated Documentation on developers.cardano.org.  
   \- Community Discussions & Q\&A via Discord, Telegram, and Twitter Spaces.

**Post-Launch**  
 • Monitor adoption through developer engagement metrics.  
 • Address early feedback and iterate on UX and feature improvements.  
 • Maintain ongoing documentation updates and learning resources.

### Metrics and KPI\*

Suggested metrics are Cycle Time, WIP, Throughput, and Work Item Age. Tracking these, the team can transform an unpredictable process into one that reliably forecasts delivery. 

These metrics provide clear insights into where delays or bottlenecks occur, enabling us to make informed, consistent predictions about when customer value will be delivered. 

* **Cycle Time:** Measures the elapsed time from when a work item starts until it is completed. This helps assess responsiveness to customer feedback. Time-to-market for a given feature.  
* **Work in Progress (WIP):** Tracks the number of active development tasks to prevent excessive work accumulation and aging of unresolved issues.  
* **Throughput:** Measures the number of work items completed per unit of time, providing insights into development speed and efficiency.  
* **Work Item Age:** Tracks the time elapsed since a work item started, helping monitor delays and identify bottlenecks in the development process. 

\* Still need to figure out how to automate these with GH. 

### Risk Management

Given that the tool is essentially a launcher, the overall risk profile is relatively low. We have identified a few key risks and associated mitigations:

• **Risk: Budget Exclusion**  
   – **Mitigation:** Leverage IO’s proven track record with cardano-node and cardano-cli testing to demonstrate the tool’s value, making a strong case for its budget inclusion.

• **Risk: Low Adoption**  
   – **Mitigation:** Engage early users with targeted developer outreach, provide clear and concise documentation, and incorporate user feedback to ensure the tool meets community needs.

• **Risk: Unexpected Bugs or Usability Challenges**  
   – **Mitigation:** Implement focused beta testing and maintain a rapid-response plan to address any issues, ensuring a smooth user experience from launch onward.

### Cardano-Testnet Roadmap

#### Phase 1: Release Version 1.0.0

**Goal:**  
 Finalize and launch Cardano-Testnet Version 1.0.0 to offer users a reliable, user-friendly tool for spinning up and managing Cardano testnets.

**Key Tasks:**

* **Align Default Protocol Parameters:**  
  * Ensure that Cardano-Testnet initializes clusters with protocol parameters mirroring Mainnet settings (excluding epoch length and slot duration).  
* **Implement Custom Output Directory:**  
  * Introduce command-line flags for users to specify an output directory for cluster artifacts, enhancing file organization.  
* **Enable Custom Genesis and Configuration Files:**  
  * Allow users to pass custom genesis and configuration files via command-line flags to support tailored testing environments.  
* **Write User Documentation:**  
  * Create comprehensive documentation covering cluster initialization, configuration requirements, and troubleshooting guidelines, to be hosted on developers.cardano.org.

**Milestones:**

* Code freeze and final internal testing.  
* Completion of documentation and user guides.  
* Official release announcement and distribution.

#### Phase 2: Gather User Feedback for Continuous Improvement

**Goal:**  
 Ensure Cardano-Testnet remains reliable, user-friendly, and aligned with community needs by actively monitoring usage and gathering feedback.

**Key Tasks:**

* **Establish Feedback Channels:**  
  * Direct the users to API/CLI  Discord channel (perhaps rename the chanel to DevTools Suite) and other feedback forums for users to share experiences and suggestions.  
* **Prioritize and Address User Feedback:**  
  * Regularly review reported issues, feature requests, and pain points to inform iterative updates and enhancements.  
* **Community Engagement:**  
  * Maintain transparent communication on updates, improvements, and roadmap adjustments through blog posts, Discord, and other community channels.  
* **Evaluate Docker Distribution:**  
  * Assess the feasibility and community demand for distributing Cardano-Testnet as a Docker container, as suggested by Sebastian Nagel.

**Milestones:**

* Regular feedback review meetings (e.g., bi-weekly) to prioritize improvements.  
* Release of instructional videos alongside Version 1.0.0  
* Decision on Docker distribution within the first quarter post-launch.

#### Phase 3: Enhance User Onboarding with Documentation and Educational Content

**Goal:**  
 Make Cardano-Testnet easy to install, use, and understand by providing high-quality educational materials and tutorials.

**Key Tasks:**

* **Produce Educational Videos:**  
  * Record and publish instructional videos demonstrating setup, customization, and advanced functionalities.  
* **Integrate into Learning Modules:**  
  * Collaborate with the Education Team to incorporate Cardano-Testnet into an IOG Cardano learning module (with input from Aiken).  
* **Gather Educational Feedback:**  
  * Collect feedback from students and instructors to refine usability and documentation further.

**Milestones:**

* Integration into the IOG Cardano learning courses (Education team) within two months.  
* Ongoing feedback collection and iterative improvement based on user education insights.