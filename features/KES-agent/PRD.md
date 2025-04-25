# KES Agent
## Product Requirements Document (PRD)

|                |                       |        
| :---           | :---                  |              
| Document owner | Carlos Lopez de Lara  |
| Responsible    | Tobias Dammers        |
| Accountable    | Carlos Lopez de Lara  |
| Consulted      | Gamze Orhon Kılıç <br/> Daniel Calatayud <br/> Nick Frisby <br/> Javier Sagredo <br/> Damian Nadales    | 
| Informed       | Samuel Leathers Ricky Rand |
| Summary        |  The KES Agent solves the problem of securely managing short-lived, forward-secure signing keys (KES keys) used by block-forging nodes in Cardano. It ensures that these keys are kept in memory only (never on disk), are securely erased after each cryptographic evolution, and remain available across process restarts (under normal circumstances). |

### Executive summary

#### Objective  
The KES agent is designed to mitigate the risk of leaked or compromised signing keys by ensuring they are stored only in memory and securely erased after each evolution. This aligns with the assumptions underpinning Ouroboros Praos, which explicitly requires forward-secure key evolving signatures with reliable key erasure to protect against adaptive corruption. Without this guarantee, an attacker compromising a stake pool could potentially forge blocks retroactively, undermining the protocol’s core security properties like common prefix and chain quality.

#### Target audience  
The primary audience is stake pool operators (SPOs) running block-forging nodes. Larger stake pools may be the most interested.

#### Vision  
By ensuring that signing keys exist only in memory and are securely erased after each evolution, the KES agent protects against retroactive forgery in the event of node compromise. It provides a practical implementation of the key lifecycle guarantees assumed by Praos, making it possible to uphold core consensus properties under adaptive adversarial conditions.

#### Strategic fit  
- Enhances Cardano’s overall security posture by mitigating the risks of KES key recovery from disk.  
- Allows SPOs to tailor their security approach based on risk tolerance and operational complexity.

---

## Goals and non-goals

### Goals
- **In-memory key management**: Ensure private keys (KES keys) never touch disk, preventing recovery of expired keys even if the node is compromised or disk data is forensically examined.
- **Forward security**: Automatically evolve (rotate) KES keys and securely erase prior evolutions.
- **High availability (HA) and self-healing**: Support multi-agent setups for minimal downtime or hot updates, especially important for larger SPOs.
- **Optional adoption**: Make the KES agent available but not mandatory. We cannot enforce it anyway.
- **Minimal downtime rotations**: Provide a mechanism for hot KES updates that do not require node restarts or cause unnecessary downtime.
- **Ease of use and clarity**: Offer clear documentation, demos, and real-world examples (e.g., SSD recovery scenarios) to showcase risk and setup.

### Non-goals
- **Windows support**: Current priority is Unix-like systems (Linux, macOS). Windows support may be considered in the future, but is not included now.
- **Seamless multi-node distribution**: There is still an overhead of configuring SSH/domain sockets.

---

## User journeys

### Example user journey: stake pool operator

- **Persona**: Stake pool operator (SPO)  
- **Goal**: Securely manage KES signing keys without storing them on disk  

**Journey**  
1. **Install & launch**: The SPO installs `kes-agent` on the same server as the Cardano node (implies issuing new OpCert at every node restart) or multiple servers for HA (recommended).  
2. **Generate new KES key**: On the same machine (or via a control host), the operator creates a new KES key in memory and outputs a verification key to disk.  
3. **Air-gapped signing**: The verification key is moved to an offline “cold key host,” which uses the cold key to sign and produce an operational certificate (OpCert), then returns the OpCert to the online environment.  
4. **Activate key**: The operator pushes the signed OpCert into the KES agent, which validates it and immediately becomes ready to distribute the new key.  
5. **Node configuration**: The Cardano node connects to the KES agent’s “service” socket on startup and requests the latest KES key for block forging.  
6. **Automatic key evolving & secure erasure**: The KES agent (and the node) evolves the key every 36 hours and erases old evolutions from memory. This hot rotation avoids a full node restart, reducing downtime.  
7. **Restart / reboot scenarios**: If the node restarts, it fetches the current key again; if the server reboots without another agent running, a new key is generated or fetched from a peer agent.

---

## High-level requirements

### Functional requirements

#### In-memory key handling  
- KES keys must be held in memory only (never written to disk). Memory holding the key should be locked to prevent swapping (mlock).

#### KES key rotation  
- The KES agent must autonomously evolve (rotate) the key every 36 hours (configurable based on Cardano protocol parameters) so that a restarting node can retrieve the corresponding key to the current KES period.

#### Secure erasure  
- Old evolutions of the KES key must be irrecoverably deleted from RAM as soon as the key is evolved, mitigating disk-recovery attacks.

#### Operational certificate management  
- Accept externally signed operational certificates (OpCerts) and validate them before activating a newly generated KES key.

#### High availability (HA)  
- Optionally support peer KES agents connecting over Unix sockets (forwarded via SSH) to replicate or fetch the latest key if one agent restarts, avoiding loss of key data.

#### CLI tools  
- Provide a CLI (`kes-agent-control`) with commands to generate new staged keys, upload OpCerts, manage multiple KES agents  
- Provide a CLI demo client for testing purposes and to demonstrate recommended setups (e.g., single-machine, Raspberry Pi)

#### Secure communications  
- Key exchange between the KES agent and the node (or peer agents) must happen over secure local sockets and never touch storage. When crossing machines, SSH forwarding (or alternatives).

### Non-functional requirements

#### Performance  
- Must handle key evolution without causing significant overhead or delays in block forging.  
- Minimize the resource utilization.

#### Reliability  
- If the node restarts, re-connection and retrieval of the current KES key should succeed consistently.  
- If the agent or server reboots, keys must be regenerated or fetched from peers.

#### Scalability  
- Setup must support single-machine SPOs with minimal overhead as well as multi-agent networks.

#### Security  
- Resist disk-recovery attacks by never writing private keys.  
- Resist accidental writes to disk (disable swap or rely on `mlock`).  
- Provide optional demos showing how data “deleted” on SSDs can be recovered, emphasizing the value of a memory-only solution.

---

## Metrics for success

### Adoption metrics  
- Number of SPOs (particularly large stake pools) using the KES agent.

### Performance metrics  
- CPU/Memory requirements of a machine running `kes-agent` to minimize cost of such machine.

---

## Dependencies and risks

| Dependency/risk              | Description                                                                                     | Mitigation strategy                                                                                 |
|-----------------------------|-------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Integration with Cardano node | Node must include client code to request or accept KES keys from the agent.                    | PR                                                                                                   |
| Server reboots (memory loss) | If the server hosting the only KES agent reboots, the key is lost.                             | Use multiple agents to self-heal (HA). Allow manual re-keying. Document that this is optional.      |
| SSH socket forwarding        | Handling secure communications between multiple machines can be complex.                        | Provide clear documentation and best practices for domain socket forwarding (via SSH or alternatives). |
| Suspend-to-disk              | Memory contents may be written to disk upon hibernation/suspend-to-disk.                        | Disable suspend/hibernate. Terminate the KES agent before hibernation. Use multi-agent to self-heal. |
| Air-gapped cold key storage  | Requires user discipline to keep cold keys truly offline (no network).                          | Clearly document offline procedures and the risk of connected cold key storage.                      |
| SPO acceptance               | Some operators may view the risk as insufficient to justify complexity.                         | Emphasize optional usage. Focus on large-delegation stake pools. Provide SSD recovery demos.         |

---

## Roadmap

### Phase 1: Core agent & basic integration
#### Deliverables

1. KES agent (daemon) with in-memory key storage, auto-evolution, and secure erasure of old keys.
2. CLI tool to generate new KES keys, staging and installing keys OpCert. 
3. Documentation. 

### Phase 2: Initial roll-out
#### Deliverables 

1. Initial adoption by 15% of large SPOs, informed by the threat model. 
2. Open for feedback and improvements

### Phase 3: Final roll out
#### Deliverables

1. KES agent available to all SPOs. 

## Definitions and glossary

| **Term**               | **Description**                                                                                                                                                                |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| KES (key evolving signature) | A cryptographic scheme allowing forward security by regularly evolving (rotating) the signing key.                                                                         |
| KES period             | The time window (e.g., 36 hours in Cardano) for which a KES key evolution is valid.                                                                                           |
| OpCert (operational certificate) | A certificate binding a KES verification key to a user’s cold key, specifying the valid start period.                                                       |
| Cold key               | Long-lived private key used to control stake pools. It is used to sign stake pool registration/deregistration certificates and stake pool operational certificates. Should be kept on cold storage, thus the name. |
| `mlock`                | A system call that prevents memory from being swapped to disk, used for securely holding sensitive data.                                                                     |
| HA (high availability) | A setup to reduce downtime, often by running multiple processes or agents that can take over as needed.                                                                      |

## Appendices and references

### Ouroboros Praos 
 - [Ouroboros Praos: An adaptively-secure, semi-synchronous proof-of-stake blockchain](https://eprint.iacr.org/2017/573.pdf)
### Threat model

- High level, should answer:

  - **Required stake or number of keys**: How many keys (or what share of stake) would an attacker need to recover before posing a serious threat?
  - **Key relevance over time**: Once MaxKesEvolutions is reached, are older keys still a risk? If KES agent adoption becomes widespread, do compromised historical keys remain a danger?
  - **Checkpoint circumvention**: Assuming an attacker can produce an alternative chain, can they bypass the checkpoint mechanisms of Ouroboros Genesis (as implemented in cardano-node 10.3.0.0)?
  - **Risk impact**: Even if the probability of such an attack is low, are the potential outcomes so catastrophic that stronger safeguards (like KES agent) become worthwhile
  - **Applicability to small SPOs**: Smaller operators, especially those producing few blocks or struggling with profitability, may be reluctant to add complexity. Is it necessary or beneficial for them to adopt the KES agent, given their relatively minor share in block production? In other words -Can we focus on larger SPOs?-

### KES agent FAQ & design:

- https://github.com/input-output-hk/kes-agent/blob/master/doc/guide.markdown
- https://github.com/input-output-hk/kes-agent/blob/master/doc/diagrams/overview.png

### SSD data recovery demo:

Reference to materials demonstrating why/how  “deleted” data may remain recoverable.

- https://youtu.be/jBsdwNZ_Fjg?si=3M6qu3Qlwu3G4eNk

## SPO Meeting notes

During our meetings with SPOs, some expressed skepticism. They view disk-based key management as “good enough” and perceive the KES agent as adding unnecessary complexity, especially for operators with low block production. It’s important to note that while the KES agent is optional, it exists to uphold one of the protocol’s most critical security assumptions—that past keys are unrecoverable. If that assumption fails, the attacker model changes significantly, and the network may become vulnerable to long-range attacks or historical chain rewrites.

- [Day 1](https://docs.google.com/document/d/16iChVUQnWp3YQHC3pjX7_-T-KmbdiNo6lfgQIubYT7M/edit?usp=sharing)
- [Day 2](https://docs.google.com/document/d/1kcltbVAVlfOO3myecK--v3tqxFor5WrFHOJO_lnPu1M/edit?usp=sharing)
