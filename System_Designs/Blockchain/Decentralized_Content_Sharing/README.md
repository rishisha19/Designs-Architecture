# Decentralized Content Sharing System Design

This solution features an infrastructure which helps curators, content creators, artists and users across the globe to share content which is verified, transparent, secured, rewardable and fast.

## Contents

- [Overview](#overview-anchor)
- [Introduction](#Introduction)
  - [Purpose and Scope](#Purpose-and-scope)
  - [Functional Requirements](#Functional-Requirements)
  - [Non-Functional Requirements](#Non-Functional-Requirements)
  - [System Overview](#System-Overview)
  - [Design Constraints](#Design-constraints)
- [System Architecture](#system-architecure)
  - [Design Choices](#design-choices)
  - [System Flows](#system-flows)
  - [Challenges and Considerations](#challenges)

## Overview <a id=overview-anchor></a>

The System Design Document describes the system requirements, operating environment, system and subsystem architecture, files and database design, input formats, output layouts, human-machine interfaces, detailed design, processing logic, and external interfaces.

## Introduction <a id=Introduction></a>

### Purpose and Scope <a id=Purpose-and-scope></a>

Purpose of this document is to provide a high level design idea for a decentralized infrastrucure.
The scope is limited to High level Design(HLD) providing a blueprint of a design choice keeping mentioned constraints in mind. The document doesn't bring focus on in depth view or low level design choices.

### Functional Requirements <a id=Functional-Requirements></a>

This new infrastructure should enable:

1. Decentralized storage of user data and account balances.
2. Verification and validation of produced data without a central entity.
3. Transparent and secure management of cryptocurrency payments.
4. On-chain tracking of all interactions (data production, verifications, votes, etc.) to ensure complete traceability.
5. User Registration and Authentication

### Non-Functional Requirements <a id=Non-Functional-Requirements></a>

1. High Availability of the environment
2. Near real time data governance
3. High throughput
4. Performant application

### System Overview <a id=System-Overview></a>

![High Level System Diagram](diagrams/decent_infra_hld.drawio.png)

The System is designed utilizing below patterns & architecure style:

- Event Driven Architecture
- Microservices
- Adapter pattern
- SOLID principles

The key areas to the architecture are

1. <b> Blockchain Resource(s): </b> The main attribute to Decentalized Apps( from here on dApp) is keeping transparency of data. Keeping data and transactions onchain utilizing smart contract technology will provide transparency, security, traceability to the system.
2. <b>Blockchain Integration Layer:</b> Integration layer provides better control & maintainibility for the code that needs interaction with blockchain(s). For example: Capturing contract events to index them for faster executions, Sending transaction to chain, etc. This layer follows adapter pattern to integrate with various chains including EVM and Non-EVM, <i>hence makes the solution dynamic and extensible</i>.
3. <b>Baceknd System :</b> To cater to users and there accounts, there content, there rewards and all information which is required to be served over App, Web or other client based methodolgy, a scalable backend system is required. The backend system is build on microservices architecure serving on chain data and other static information to user in quick time without making expensive calls to chains or other resources.
4. <b>App & Web Layer: </b> A Front end layer which connects to backend system or blockchain for send transaction or retrieving data to serve the end user.

### Design Constraints <a id=Design-constraints></a>

There can be maitainance overhead for maintaining different adapters, but that is a tradeoff to be more dynamice and scalable to n chains.
Whole system is more close to on chain event and does a lot of processing based on on-chain data. There are chances of these events become irregular and can become challenging. But, it makes the whole system more real time and in sync with onchain.

## System Architecture <a id=system-architecure></a>

The whole system architecture is divided into 3 important aspects:

- <i>Decentralized Infrastructure </i>:
  - To store content shared by users in a transparent ways
  - To Validate, verify and track of content added/modified, votes, rewards etc.
  - To transfer rewards
- <i>An Event Driven Backend System</i>: The bacekdn system helps as an auxialry services to help providing faster access to data for better user expereiences. Being event diven along with microservice architecture in nature helps make the whole system modular, scalable and adapt to changing business needs more swiftly.
- <i>Mix of Web3 & Web2 Coomunication</i>: Use of Web3 involves browser based encrypted communication to on chain transaction which is then supported by traditional Web2 technologies like HTTP, REST, Pub/sub based mechanisms helps in making communication secured and advanced.

### Design Choices <a id=design-choices></a>

1. <i>Blockchain Network(s)</i>:

    The solution requires custom validators and consensus mechanism to help verify the authenticity of content published by the creators and validated results are also available publicly for transparency.

    Based on the soultion requirement, I would consider <i>**rollups preferably celestia**</i>. Which helps in doing custom validation and cerification off chain and publish the results to L1 chain. Keeping both transparency and authenticity at the core. This helps in achieving scalability, flexibility, security, speed, efficency more quickly and implictly keeping the solution decentralizated and transparent.

2. <i>Data Storage</i>: For building a fully decentralized infrastructure that eliminates the middleman and increases trust, we need to carefully select storage solutions that meet below requirements:

    - Decentralized storage: Data should not reside in a single location.
    - Immutability: Data should be tamper-proof.
    - Verifiability: Data integrity and authenticity should be verifiable.
    - Transparency: Data should be accessible for auditing and verification.
    - Efficiency: Data access and retrieval should be efficient.
  <br></br>

    Given the requirements, a hybrid approach combining multiple decentralized storage technologies is optimal:

    <h4>Combining IPFS and Arweave</h4>
    IPFS

        - Decentralized: Data is distributed across a network of nodes.
        - Immutable: Data is content-addressed, meaning it is retrieved using a unique hash, ensuring data integrity.
        - Efficient: Only the necessary data is retrieved, reducing redundancy.
        - Wide Adoption: Well-supported by the decentralized application (dApp) ecosystem.
    Arweave:

        - Permanent Storage: Once data is stored, it is permanent and immutable.
        - Decentralized: Data is distributed across a network of nodes.
        - Cost-Efficient in the Long Run: One-time payment for permanent storage.
        - Scalability: Designed to handle large-scale data storage needs.

    Decentralized Storage of User Data and Account Balances:

    - IPFS provides decentralized, content-addressable storage, ensuring data integrity.
    - Arweave ensures that critical data is permanently stored and immutable, enhancing security and trust.
    <br></br>

    Verification and Validation of Produced Data:

    - IPFS allows verifiers to access the data efficiently using content hashes.
    - Arweave ensures that verification results and related metadata are permanently stored and immutable, supporting transparent validation processes.
    <br></br>

    Transparent and Secure Management of Cryptocurrency Payments:

    - Use smart contracts on a blockchain (e.g., Polygon) to manage payments securely.
    - Store transaction metadata on Arweave for permanent, tamper-proof records.
    <br></br>

    On-Chain Tracking of All Interactions:

    - Record all interactions (data production, verifications, votes, etc.) on the blockchain for complete traceability.
    - Use IPFS to store detailed interaction data, with references (content hashes) stored on the blockchain.
    - Ensure critical interaction records are also stored on Arweave for permanence.
  <br></br>

    Along with this a <i>SQL Database like PostgresSQL</i> can be used, to index on chain data, store event information, store other necessary information to effeciently serve our FE client.
    <br></br>

3. Verifier Selection and Consensus Mechanism

    <h4>Verifier Selection</h4>
    Selecting reliable verifiers is crucial for the integrity of your decentralized system. Here are some potential strategies:

    | Strategy | Pros | Cons |
    | :---   | :---  |  :--- |
    | **Random Selection**       | Simple to implement, ensures fairness.                    | Potential for selecting low-quality verifiers, susceptible to Sybil attacks. |
    | **Reputation-Based Selection** | Prioritizes verifiers with good track records, improves system reliability. | Requires a reputation system, susceptible to reputation manipulation. |
    | **Stake-Based Selection**  | Aligns verifier incentives with system success, resistant to Sybil attacks. | Requires a staking mechanism, potential for wealth concentration.  |
    | **Hybrid Approach**        | Combines benefits of different methods, enhances system robustness. | Increased complexity.|

    Given the requirements of scalability, fairness, and decentralization, <i><b>a hybrid approach combining random selection and reputation-based selection seems optimal</i></b>. This balances the need for fairness with the desire for quality verifiers.

   - **Random selection** ensures fairness and prevents centralization.
   - **Reputation-based** selection improves system reliability by prioritizing experienced verifiers.

    <h4>Verifier Validation and Consensus Mechanism</h4>

    Validating verification results and reaching consensus among verifiers is fundamental to the system's integrity. Key mechanisms include:

    | Strategy | Pros | Cons |
    | :---   | :---  |  :--- |
    | **Proof of Work (PoW)**           | Strong security guarantees, resistant to double-spending.  | High energy consumption, centralization risks.                    |
    | **Proof of Stake (PoS)**          | Lower energy consumption, faster block times.              | Potential for "nothing at stake" problem, vulnerability to centralization. |
    | **Delegated Proof of Stake (DPoS)** | Faster block times, improved scalability.                  | Potential for centralization, susceptible to attacks.             |
    | **Practical Byzantine Fault Tolerance (PBFT)** | Fast consensus, high fault tolerance.                         | Limited scalability, requires a fixed number of validators.       |

    To achieve scalability, efficiency, and decentralization, <i><b>a Proof of Stake (PoS) or Delegated Proof of Stake (DPoS) consensus mechanism is preferable</b></i>. These mechanisms offer faster block times and lower energy consumption compared to Proof of Work.

    - **PoS** aligns verifier incentives with system success, promoting system stability.
    - **DPoS** can offer even faster block times but requires careful monitoring to prevent centralization risks.

### System Flows <a id=system-flows></a>

<h4>User Registration</h4>

- **Key Pair Generation:** Users generate a public-private key pair for secure identification.
- **Distributed Identity(DID) Registration:** Public keys are registered on a decentralized identity network such as Sovrin.
- **On-Chain Storage:** A hashed representation of the public key is stored on a shard of the main blockchain.
- **Off-Chain Profile Storage:** User profile data, excluding sensitive information, is stored off-chain in a distributed storage network in IPFS, with its hash recorded on-chain.

<h4>Content Creation/Data Submission</h4>

- **Content Creation:** Users create content or data and store it in a distributed storage network.
- **On-Chain Metadata:** A content hash and metadata (e.g., creation time, author) are stored on-chain within a rollup.
- **Random Shard Assignment:** Content is assigned to a random shard for verification.

<h4>Verification</h4>

- **Random Verifier Selection:** Verifiers from different shards are randomly selected to verify the content.
- **Off-Chain Verification Results:** Verification results (pass/fail, reasons) are stored off-chain in a distributed storage network.
- **On-Chain Summary:** A summary of verification results (e.g., number of passes, fails) is stored on-chain within the rollup.

<h4>Voting</h4>

- **Vote Casting:** Users cast votes on verified content.
- **Off-Chain Tallying:** Votes are tallied off-chain and stored in a distributed database.
- **On-Chain Voting Summary:** A summary of voting results (like total votes, outcome, etc.) is stored on-chain within the rollup.

<h4>On-Chain Recording and Distribution</h4>

**Rollups**

- Scalability: Rollups handle the majority of transactions off-chain, significantly improving scalability and reducing transaction fees.

**Sharding**

- Decentralization and Performance: Data and processing are distributed across multiple shards, enhancing both decentralization and system performance.

**Distributed Ledger**

- Critical Metadata Storage: The main blockchain acts as a distributed ledger, recording critical metadata and rollup summaries.

**Cross-Shard Communication**

- Coordination Mechanisms: Implement robust cross-shard communication mechanisms to coordinate verification and voting processes.

**Consensus**

- Distributed Consensus Algorithm: Utilize a distributed consensus algorithm, such as Proof-of-Stake (PoS), to ensure agreement among nodes and maintain network integrity.

<h4>Features</h4>

**Distributed Storage**

- IPFS & Arweave Utilization: Use of IPFS and Arweave decentralized storage solution forUser profile,  content and verification results.

**Distributed Verification**

- Shard-Based Verification: Verification tasks are distributed across multiple shards, ensuring no single point of failure.

**Distributed Voting**

- Secure Voting System: Employ a distributed voting system to enhance security and decentralization.

**Sharding**

- Scalability: Partition the blockchain into multiple shards to improve scalability and system performance.

**Consensus**

- Decentralized Integrity: Maintain network integrity using a decentralized consensus algorithm.

### Challenges and Considerations <a id=challenges></a>

**Network Latency**

- Impact on Real-Time Operations: Distributed systems can introduce latency, which may impact real-time operations.

**Data Consistency**

- Complexity: Ensuring data consistency across multiple shards can be complex and requires sophisticated coordination mechanisms.

**Security**

- Robust Measures: Protecting against attacks on distributed systems necessitates robust security measures and continuous monitoring.

**Complexity**

- Operational Overhead: Managing a distributed system involves increased operational complexity and overhead.

By carefully addressing these challenges and leveraging the strengths of rollups and sharding, this architecture can provide a highly decentralized, scalable, and secure platform for a wide range of applications, fostering trust and reliability among users and stakeholders.
