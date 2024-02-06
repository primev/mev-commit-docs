# Primev Docs Testnet Ver.

# Welcome

## **Get Started**

### **I. Searchers (Bidders)**

For those actively engaged in identifying and capturing MEV opportunities, Primev provides advanced tools and insights for effective bidding and interaction with the Ethereum blockchain.

- mev-commit: A Deep Dive
- Bidding Process and Structure
- API Integration for Bidders
- Network Topology and Protocols
- System Requirements for Bidders

### **II. Ethereum Users**

General users of the Ethereum network can benefit from Primev's focus on transaction efficiency and MEV-related insights.

- Understanding mev-commit
- Bids and Privacy
- FAQs

### **III. Validators, Block Builders & Developers (Providers)**

Validators, builders, and developers play a crucial role in the Primev ecosystem. Explore how Primev's tools can enhance block production and validation processes.

- Provider's Role in mev-commit
- Setting Up a Provider Node
- API Integration for Providers
- System Requirements for Providers
- Contracts and Security Best Practices

## Introduction to Primev

### What is Primev?

**Primev** represents a pioneering development in blockchain communication, focusing on the blockbuilding process and execution bids within Ethereum's ecosystem. As a domain-agnostic network, Primev aims to illuminate and streamline the complex interactions among various actors in the MEV (Miner Extractable Value) pipeline. Primev network aims to solve coordination inefficiencies that continue to arise in blockchain ecosystems as more roles are introduced into systems such as the mev pipeline and more solutions arise for increased decentralization and censorship resistance.

Aligned with the ethos of the crypto community and Ethereum's foundational vision, Primev is not just a tool but a strategic facilitator. It bridges the gap between users' expectations and the blockchain's ability to meet them, enhancing user experience and confidence in blockchain interactions. Primev's approach is about empowering users to communicate their needs clearly and monitor the fulfillment of these needs, ensuring their actions are based on reliable and timely information.

### mev-commit

Our first product, **mev-commit**, a key component of Primev, is a peer-to-peer networking software designed for real-time interaction between various mev actors. It enables the exchange of execution bids and commitments, offering a more granular and dynamic transaction execution experience.

### Contribute

Stay updated and engage with Primev through our social platforms. Follow us on [Twitter](https://twitter.com/primev_xyz) for the latest news and updates, and contribute to our open-source journey on [GitHub](https://github.com/primevprotocol/).

## Understanding mev

(links from below page) + any other references

[MEV Resources](https://www.notion.so/MEV-Resources-a3b3d108c3c64aac8d0f398de5a3cfdc?pvs=21)

# mev-commit

## Overview

### What is mev-commit?

Mev-commit is a peer-to-peer (P2P) networking platform designed to facilitate direct and real-time interactions between mev actors and execution providers. It provides a robust network for exchanging execution bids and commitments, thus significantly improving the transaction execution process. This platform allows for detailed specification of execution requirements and delivers timely and reliable commitments, ensuring a more precise and efficient transactional experience.

![Untitled](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Untitled.png)

### Why mev-commit?

**mev-commit** addresses the centralizing tendencies of mev within Ethereum's ecosystem. Unchecked, mev can lead to consensus instability and the emergence of permissioned communication infrastructures among searchers, block producers, and validators.

In a PoS (Proof of Stake) Ethereum landscape, where block subsidies are expected to reduce, mev's proportion in total staking revenue becomes significantly more impactful. mev-commit offers a decentralized solution, enabling real-time, transparent exchanges between various mev actors. It facilitates a fairer, more accessible mev environment, fostering a more stable, efficient, and equitable blockchain ecosystem. By leveraging mev-commit, actors can engage in mev opportunities while contributing to the overall health and decentralization of Ethereum.

### How does mev-commit work?

At its core, mev-commit establishes a dynamic and efficient conduit for MEV actors, such as searchers and block builders, to exchange execution bids and commitments. The process begins with MEV actors joining the mev-commit network, where they can broadcast and receive bids in real-time. These bids, encapsulating transaction details and proposed execution fees, are communicated across the network. Upon receipt, execution providers evaluate these bids and may issue binding commitments if they choose to include the transactions in an upcoming block. This mechanism not only ensures a transparent and competitive environment for MEV extraction but also contributes to a more stable and equitable blockchain ecosystem by prioritizing clarity and fairness in transaction execution

Mev-commit's p2p network is structured to allow real-time communication across network actors:

![Untitled](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Untitled%201.png)

**Connections:** Users and providers are interconnected, with each node initially accessing a primary network node (bootnode) for startup.

**Gateway Nodes:** Providers can set up gateway nodes, allowing bid submissions to their mev-commit RPC endpoint first. These bids will also be gossiped amongst providers.

The diagram below illustrates how **bids, commitments, and funds** flow with mev-commit, reflecting its efficiency in facilitating P2P interactions for mev actors.

![https://docs.primev.xyz/flow.png](https://docs.primev.xyz/flow.png)

## mev-commit Details

## System Architechture Overview

![System Overview - b2.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/System_Overview_-_b2.png)

The system architecture of mev-commit is composed of three primary components:

- **API**:
  - Features a gRPC/HTTP interface for main API interactions.
  - Includes debug endpoints to access metrics, pprof, and other relevant information.
- **P2P Node**:
  - Operates as a libp2p node, utilizing custom protocols for network communication.
- **Settlement Client**:
  - Functions as a wrapped Ethereum client, responsible for issuing and managing transactions.

### mev-commit P2P Node

The mev-commit P2P network comprises three distinct types of nodes:

- Bootnode
  Bootnodes are nodes managed by the Primev team that provides service discovery. Bootnodes only run a subset of the p2p protocols that are responsible for creating connections and gossiping peer information across the network to achieve full connectivity between the different nodes.
- Bidder node
  Bidder nodes create signed bids and broadcast them to all the provider nodes on the mev-commit network and receive the signed commitments from the providers. Providers can accept/reject bids. The bidders will get commitments only for the accepted bids.
- Provider node
  Provider nodes will receive the signed bids from the bidders. They will communicate with the provider block-building infrastructure and decide whether they will accept the bid or not. Providers will accept bids if they have decided to include the transaction in the block that they are planning to propose. The commitments are also sent to the mev-commit settlement chain where they will be settled once the block is built on the L1 chain.

**Core Protocols in P2P Node**

The P2P node implements key protocols to facilitate interaction among different actors. Currently, there are 3 main protocols:

- **Handshake**
  When the nodes initially connect, they exchange signed messages with each other to authenticate themselves. They are allowed to participate in the other protocols only after the successful conclusion of the handshake. - Provider nodes need to be **staked** in the provider registry before they can connect to other nodes.

![Handshake.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Handshake.png)

- **Discovery**
  To establish connections with other actors, the nodes need to gossip underlay information. The discovery protocol is used to achieve this. Currently, the network tries to establish a mesh topology; where all the nodes are connected.

      ![bootnode.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/bootnode.png)

- Preconfirmations
  Once the nodes establish connectivity, they will start interacting with other actors on the preconfirmations protocol to exchange signed bids and commitments.
      ![mev p2p.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/mev_p2p.png)

## Actors

Network actors' roles are defined based on their interactions with other ecosystem actors. A diagram below depicts a given mev actor's relative placement compared to others.

![mev-supply-chain.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/mev-supply-chain.png)

For example, a Searcher is a bidder for a Sequencer; but that same Sequencer can be a bidder for a block builder. Thus it's best to think of actors' roles in mev-commit similar to their roles in the mev pipeline. To the left of the diagram are bidders, and to the right of the diagram are execution providers who can issue commitments against these bids.

Under PBS, information only moves to the right amongst actors in the mev pipeline. With mev-commit, credible commitments for execution that share bits of information flow from providers back to bidders, who consume blockspace.

### **Providers**

Providers of execution services, such as **Block Builders and Rollup Sequencers**.

### **Bidders**

Users bidding for execution services include **mev searchers, AA bundlers, solvers, Rollup Provers, and others seeking efficient use of block space**.

## Bids

The current bid structure being sent to the internal API is as follows:

```bash
# Example Bid Structure:
# txn_hash: arbitrary string value representing txn hash
# bid_amt: amount to bid in wei
# block_number: target block to have bid included
# bid_digest: hash of the above bid keccak{txn_hash, bid_amt, block_number}
# bid_signature: Signed bid_digest = sign(bid_digest, signing_key)
{
   "txn_hash":"0x8d562df4f0363de75c62441d69c46aa6c9144a6e9e4697509b3b24bf8a694322",
   "bid_amt":277610,
   "block_number":2703,
   "bid_digest":"uU2p20f5KmehWqpuY1u+CbhcS8jNwdQAJQe2dh0Vnrk=",
   "bid_signature":"nY6jYsGPxj6LVlSVQJbZcxvmRrw8Ym5rqOL1x0W/xPlJGBaF/ZzzjkxiioY/MDiRGvlflSWeoT0fh3aIJiJxAhw="
}
```

The bid structure also includes a hash and signature that is auto-constructed by the mev-commit bidder node.

Note that the prepay amount a bidder has set needs to be 10 times higher than the bid_amt in the bid. This is expected to change in the future with the use of Account Abstraction for Bids.

## Commitments

Commitments represent promises made by providers in a cryptographically verifiable manner.

```bash
# commitmentDigest: Represents the hash of the bid structure, including the bid signature
# commitmentSignature: Represents an ECDSA signature of the [commitmentDigest]
{
   "result":{
      "txHash":"91a89B633194c0D86C539A1A5B14DCCacfD47094",
      "bidAmount":"2000",
      "blockNumber":"890200",
      "receivedBidDigest":"61634fad9081e1b23a00234a52a85386a91fc0fad3fb32d325891bdc4f8a52a9",
      "receivedBidSignature":"be99f499be5483665d569bd52fa8c4c9ed5c3bed2f94fb9949259bf640d8bd365b1d9a4b9ccb3ace86ffdcd1fdadc605b0f49b3adaf1c6a9ffea25585234664f1c",
      "commitmentDigest":"08a98d4c9d45f8431b46d99a23e6e8f82601ccf0773499d4e47bcd857cad92a6",
      "commitmentSignature":"cfc1e4e2ea9cc417027ed5846fa073c8e6031f70f91ffa9b92051b5b7738c0500d8fefb0fc189bce16004b3c0e1c4cfa41260968161adf8b55446ed5c40d1ac51b"
   }
}
```

The commitment consists of the details associated with the bid along with signatures made by the provider.

These commitments are stored on the mev-commit chain by the provider and also sent back to the originating bidder via the mev-commit p2p network.

## Privacy

Privacy in blockchain transactions is crucial, and mev-commit directly addresses this. It protects bidders' strategic interests by keeping bid and commitment details confidential. This approach is vital in preventing potential manipulations in the auction process. mev-commit's privacy features ensure that sensitive information is only revealed when necessary, maintaining transaction integrity and secure decentralized finance principles.

### Bids and Privacy

mev-commit is inherently pseudonymous, allowing any Ethereum address to submit a bid for the execution of any transaction, including bids for transactions that belong to others. Bids are visible to providers on the network and are identifiable by transaction hashes. Bids get processed by network providers and mev-commit chain validators for verifiable commitments and seamless reward settlement. mev-commit will develop bid privacy with stronger cryptographic guarantees over time.

### Commitments and Privacy

Commitments are signatures from providers accepting bids. mev-commit offers standard and privacy-preserving commitment methods for providers to choose from. Privacy-preserving commitments safeguard the confidentiality of commitment details, letting only the bidder know their bid is committed to during the block slot, and revealing the full details for the network after the slot concludes. mev-commit will have privacy preserving commitments in a subsequent release during the testnet phase.

You can read more about how mev-commit enables execution commitment privacy [here](https://mirror.xyz/0xB456F9deb9bB6f545f91Ce2949C458c3A723659e/1gjUCw9tCUDZ2U71N-6IkINeJYyhuTdC7WeVeBam-fM).

## mev-commit Chain

### **mev-commit chain connection details**

### **Services links**

| Service           | Link                        |
| ----------------- | --------------------------- |
| Bootnode endpoint | http://34.215.163.180:8545/ |
| Bridge frontend   | http://34.215.163.180/      |
| Block explorer    | http://34.209.10.199/blocks |

### **Contract addresses**

| Contract               | Address                                    |
| ---------------------- | ------------------------------------------ |
| UserRegistry           | 0x390066a15e1048445F1B1b69Ba98AC4cb5e91c52 |
| ProviderRegistry       | 0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC |
| PreConfCommitmentStore | 0xBB632720f817792578060F176694D8f7230229d9 |
| Oracle                 | 0x51dcB14bcb0B4eE747BE445550A4Fb53ecd31617 |
| Whitelist              | 0xc5bB85F941fb8dbbed6416A8aC84A06226E0f138 |

### **Current design**

The mev-commit chain is currently built out as an Ethereum sidechain run with [go-ethereum’s Clique proof-of-authority consensus mechanism(opens in a new tab)](https://geth.ethereum.org/docs/tools/clef/clique-signing).

### **Level of decentralization**

Today, most or arguably all Ethereum scaling solutions rely on centralized bridging and sequencing. Our system components rely on existing tech, and consequently inherit some centralization. However, we’ve chosen solutions that allow anyone to permissionlessly validate correct execution, and operation of chain infrastructure.

To begin with, Primev entities will run all validating infrastructure for the mev-commit chain, where correct/honest operation can be permissionlessly audited. Spinning up a full node and connecting to the mev-commit chain as a peer is encouraged. It’s also encouraged for anyone to run their own bridge relayers. Over time we can permit entities outside of Primev to become POA signers or bridge validators.

The mev-commit chain will continue to evolve. Open source scaling solutions that prove to become practical, decentralized, and/or provably secure will be utilized.

### **POA geth nodes**

Primev currently maintains one bootnode who doesn’t participate in consensus, and a set of fullnode POA signers. These signers take turns proposing the next block via a waiting period.

In order for mev-commit’s reward mechanism to be granular enough, the mev-commit chain must be able to commit blocks at a much faster rate than L1 Ethereum. We’ve chosen a target block period of 200ms. Thus, on average, 60 blocks will be committed on the mev-commit chain for every Ethereum mainnet block.

Future experimentation will help identify the maximize the number of signers that can feasibly achieve our 200ms block period constraint. Additionally, we'll be investigating the impact of geographical distance between signers on network latency.

### **Contracts**

Contracts are deployed on the mev-commit chain to follow the state of bids and commitments, and invoke rewards or slashing as needed. Contracts are designed as follows:

- A pre-confirmation contract allows pre-confirmation commitments to bids from the p2p network to be tracked on-chain.
- Two separate registry contracts exist to manage users and providers, where both parties must stake ETH to participate. Rewards and/or slashing are managed by these contracts.
- An oracle contract receives L1 payloads from the oracle service. To start, it will receive data from L1 Testnet Sepolia and transition to Mainnet as we go from Testnet to Mainnet.
- A whitelist contract allows certain other contracts to mint/burn native ether as a part of bridging with L1.

### **Oracle service**

The oracle service is an off-chain process that interacts with the oracle contract as needed. This service monitors and extracts the winning builder and corresponding transaction hash list from each L1 block, and submits this data to the oracle contract residing on the mev-commit chain.

Although this oracle is currently centralized and operated by Primev, it can eventually be integrated into the mev-commit chain validation protocol, and secured by the same federated actors that operate the mev-commit chain.

# Oracle

**_The primary role of the oracle is to ensure that the provider carries through on their commitments._**

The oracle consists of two parts:

- A smart contract component [here](https://github.com/primevprotocol/contracts/blob/main/contracts/Oracle.sol).
- A microservice [here](https://github.com/primevprotocol/mev-commit-oracle).

![oracle-detailed-overview.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/oracle-detailed-overview.png)

The oracle mechanism in this protocol automates the verification of provider commitments to include specific transactions in L1 blocks. The Oracle Microservice queries the L1 Data Source for transaction hashes and extra data, cross-references these with commitments logged in the Commitments Contract, and then the Oracle Contract verifies adherence. If a commitment is met, the Oracle Contract triggers a reward to the provider via the Bidder Contract; if not, it slashes the provider by interacting with the Provider Contract.

## Prior to Oracle Engagement

Before the oracle can be engaged according to the protocol's intended sequence of events, several preliminary actions must be taken:

![Oracle-before.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Oracle-before.png)

1a. A bidder sends their raw transaction to a provider, such as a Block Builder or the public mempool.
1b. Simultaneously, the bidder issues a bid via the mev-commit peer-to-peer network.

After the bidder's actions, the provider proceeds to:

2a. Retrieve the bid from the MEV-commit network.
2b. Issue a commitment for the transaction mentioned in the bid.

## Oracle Happy Path Flow

When a bid for a transaction (txnj) is circulated through the mev-commit p2p network and the provider has made a commitment, the oracle is activated to verify whether the provider has honored this commitment.

![Oracle.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Oracle.png)

The process is as follows:

1. The provider assembles the complete L1 block, incorporating txnj, to which it previously committed. **Note: If the provider doesn’t get selected to construct the L1 Block, the oracle flow does not get triggered.**
2. The Oracle microservice fetches the block from the L1 chain and verifies the presence of txnj.
3. Upon confirmation that txnj is included, the microservice notifies the oracle smart contract of the commitment's fulfillment.
4. The oracle smart contract then initiates the transfer of funds from the bidder's prepaid balance to the provider's account.

## Oracle Slashing Path

In the event that a provider fails to include a committed transaction (txnj) in the L1 block, the Oracle Slashing Path is initiated to penalize the provider for the breach of commitment. This process is outlined below:

![Oracle non happy.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Oracle_non_happy.png)

1. The provider is expected to produce an L1 block that includes the committed transaction (txnj). However, if the provider does not include txnj, the protocol moves to the slashing phase.
2. The Oracle microservice retrieves the block from the L1 chain and searches for txnj. If the transaction is not found, it triggers the next step.
3. The Oracle communicates that txnj was not seen in the Oracle Contract.
4. The Oracle Contract, upon receiving this information, initiates the slashing procedure. This involves penalizing the provider by triggering a slash event, which results in the forfeiture of the stake held by the provider contract as a guarantee of honest behavior.
5. The confiscated stake is subsequently allocated to the bidder as compensation for the provider's failure to include the committed transaction, thereby ensuring that the bidder is recompensed for the provider's breach of trust.

## **Provider Attribution Methodology**

The current methodology for identifying the provider (also known as the builder) responsible for constructing the L1 block relies on the **`extra_data`** field within the block's execution payload. We correlate this information with the corresponding address registered in the mev-commit protocol. There are ongoing efforts to develop and implement more cryptographically secure methods of attribution.

## Oracle Roadmap

In the longer term, we plan to enable stronger provider attribution through the use of shortened signatures in the extra-data payload.

## Slashing Conditions

**Unsatisfied promise**: In the condition where a provider was meant to carry out a promise, and was able to carry out the promise (e.g was able to deliver payload into target slot), but refused to do so.

### **Bridge to Sepolia**

The mev-commit chain is bridged to Sepolia via a [hyperlane warp route(opens in a new tab)](https://docs.hyperlane.xyz/docs/protocol/warp-routes), involving multiple agents delivering or validating cross-chain messages.

Users initiate a bridge transaction on L1 by locking ether in the bridge contract. A permissioned validator sets monitors for these transactions and attests to their merkle inclusion on L1. A permissionless relayer agent then delivers the message to the mev-commit chain’s `Interchain Security Module` (ISM), which verifies validator signatures and mints native ether on the mev-commit chain as needed.

Bridging ether from the mev-commit chain back to L1 follows a similar process, except that native ether is burned on the mev-commit chain, and unlocked on L1.

Hyperlane exposes a contract interface that allows bridge users to pay ether to cover the costs of delivering a message on the destination chain. See [Interchain Gas Payment(opens in a new tab)](https://docs.hyperlane.xyz/docs/protocol/interchain-gas-payment) for more details.

Running a hyperlane relayer is permissionless, and we encourage anyone to run their own relayer relevant to the mev-commit chain bridge. See [Running Relayers(opens in a new tab)](https://docs.hyperlane.xyz/docs/operate/relayer/run-relayer) for more details.

## System Requirements

### mev-commit software components

- [mev-commit client](https://github.com/primevprotocol/mev-commit)
- [mev-commit-geth](https://github.com/primevprotocol/mev-commit-geth)
- [contracts](https://github.com/primevprotocol/contracts)
- [mev-commit-oracle](https://github.com/primevprotocol/mev-commit-oracle)
- [mev-commit-bridge](https://github.com/primevprotocol/mev-commit-geth/tree/master/geth-poa)
- curl

### mev-commit execution requirements

- Software: mev-commit client and curl
- OS: 64-bit Linux, Mac OS X 10.14+
- CPU: 4+ cores @ 2.8+ GHz
- Memory: 16GB+ RAM
- Network: 1 GBit/sec broadband
- Disk Space: min 100 gb

## Run mev-commit

mev-commit software operates as a command-line interface (CLI) tool, designed for users to run in their own environments. The CLI mainly includes two commands. To view available commands, use the `-h` or `--help` option with the main command.

Example:

```sql
❯ mev-commit -h
NAME:
   mev-commit - Entry point for mev-commit
USAGE:
   mev-commit [global options] command [command options] [arguments...]
VERSION:
   "v1.0.0-alpha-a834960"
COMMANDS:
   start       Start the mev-commit node
   create-key  Create a new ECDSA private key and save it to a file
   help, h     Shows a list of commands or help for one command
GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```

### If you're not using Docker:

1. **Install `[buf](https://buf.build/docs/installation)`**:
   - Follow the installation instructions for **`buf`** specific to your operating system.
2. **Generate Required Files**:
   - Open your command line and run:
     ```
     Copy code
     buf generate

     ```
3. **Compile mev-commit**:
   - In the command line, execute:
     ```go
     goCopy code
     go build -o mev-commit ./cmd/main.go

     ```

### If you're using Docker:

1. **Build Docker Image**:
   - Open your terminal and navigate to where your Dockerfile is located.
   - Run:
     ```sql
     sqlCopy code
     docker build -t mev-commit:latest .

     ```

## Cryptographic Stack

### Signature Schemes

We implement ECDSA (Elliptic Curve Digital Signature Algorithm) keys for our signature process. Additionally, our messages are formatted using EIP-712 standards, enhancing human readability and structured data representation. The ECDSA framework enables the recovery of both the public key and the signer's address through the ECRecover function, which are native to Geth.

### Hashing

Our system employs keccak256 (SHA-3) for hashing, aligning with EVM standards for optimal interoperability. This is useful for interoperability with the EVM, as it natively supports keccak256 through pre-compiles, both on L1 Ethereum and our mev-commit chain.

## FAQ

- What is a Prepay amount?
  The prepay amount, or allowance, is a deposit bidders make to enable bidding. For instance, to place 10 bids of 100 gwei each, a bidder would deposit 1000 gwei as a prepay. This helps the provider monitor preconfirmed bids and ensures bidders don't exceed their specified allowance.
- I’m getting the following error: “failed to read msg: stream reset”
  If you’re a provider node, this is likely due to a lack of staking and registration on the provider contract. Follow the instructions under _Step 7. Stake Funds and Register,_ to resolve this issue.
- Why does cast need to be used?
  Cast is used in various situations to allow users to interact with the mev-commit chain. However, you can feel free to use any EVM-compatible tooling and set the RPC URL accordingly.
- What is Foundry?
  Foundry is a suite of tools that allows for both smart contract development and interactions. It’s used extensively for the deployment, testing, and interactions with mev-commit contracts.
- Do I need to install Cast?
  No, however, it can make doing things like transferring funds for interacting with smart contracts easier.
- Why do I get the same commitment back when I resend a bid?
  This is because the commitments work based on hashes and ECDSA signature schemes. These schemes are deterministic, such that as long as the bid payload and bidder private key remains the same, and the provider private key remains the same, the output commitment will be the same, even if the bid is sent more than once.

# mev-commit Testnet

## Testnet Overview

### Introduction to Testnet and its Purpose

The testnet allows bidders and providers to spin up nodes and send bids and commitments along. This will allow bidders to get pre-confirmations from providers on the Holesky network.

### Differences between the testnet and mainnet environments

Testnet will use Holesky and Holesky eth, whereas mainnet uses mainnet eth. Commitments are also written to the settlement in plaintext, whereas mainnnet will include private commitments.

### Limitations of the testnet

We do not yet support a decay mechanism that would allow for higher rewards to accrue to a provider when they submit a commitment earlier into a slot.

We also do not have bid and commitment privacy beyond the encrypted channels that exist in the p2p communication across the network. This means that when a commitment and bid are stored on the chain, they’re public.

### How users can provide feedback or report issues encountered on the testnet?

Users can open an issue on the GitHub repository that is associated with any issue they encounter. They can also tag @primev_xyz on Twitter.

## Infrastructure

### **Overview**

Primev will maintain a testnet consisting of the mev-commit chain, oracle service, and hyperlane bridge to Sepolia.

![https://docs.primev.xyz/chain.png](https://docs.primev.xyz/chain.png)

## Testnet System Requirements

Please refer to System Requirements section [here.](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8.md)

## \***\*Configuration\*\***

1. Run mev-commit using the instructions [here.](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8.md)
2. **Generate ECDSA Private Key**:

   - An ECDSA private key is required to create an Ethereum address for the node as well as to use for the P2P network. Bidders can add an existing key or create a new key using the `create-key` command.

   ```sql
   NAME:
      mev-commit create-key - Create a new ECDSA private key and save it to a file

   USAGE:
      mev-commit create-key [command options] <output_file>

   OPTIONS:
      --help, -h  show help
   ```

3. **Create a YAML Configuration File**:

   - Use the examples in the **`config`** folder of the project as a reference.
   - Once the key is available, create a yaml config file. Example config files are available in the [config](https://github.com/primevprotocol/mev-commit/tree/main/config) folder. The important options are defined below:

   ```sql
   # Path to private key file.
   priv_key_file: ~/.mev-commit/keys/nodekey

   # Type of peer. Options are provider and bidder.
   peer_type: provider

   # Port used for P2P traffic. If not configured, 13522 is the default.
   p2p_port: 13522

   # Port used for HTTP traffic. If not configured, 13523 is the default.
   http_port: 13523

   # Port used for RPC traffic. If not configured, 13524 is the default.
   rpc_port: 13524

   # Secret for the node. This is used to authorize the nodes. The value doesnt matter as long as it is sufficiently unique. It is signed using the private key.
   secret: hello

   # Format used for the logs. Options are "text" or "json".
   log_fmt: text

   # Log level. Options are "debug", "info", "warn" or "error".
   log_level: debug

   # Bootnodes used for bootstrapping the network.
   bootnodes:
     - /ip4/35.91.118.20/tcp/13522/p2p/16Uiu2HAmAG5z3E8p7o19tEcLdGvYrJYdD1NabRDc6jmizDva5BL3

   # The default is set to false for development reasons. Change it to true if you wish to accept bids on your provider instance
   expose_provider_api: false
   ```

4. **Start the mev-commit Node**:
   - Place your config file in a **`.mev-commit`** folder in your home directory.
   - Once this is done, users can start the node using the `start` command.
     ```sql
     NAME:
        mev-commit start - Start the mev-commit node

     USAGE:
        mev-commit start [command options] [arguments...]

     OPTIONS:
        --config value  path to config file [$MEV_COMMIT_CONFIG]
        --help, -h      show help
     ```
5. **Optional: Monitor Node Status**:

   - Use **`/topology`** endpoint on the HTTP port to check peer connections.

   ```
   {
      "self": {
         "Addresses": [
            "/ip4/127.0.0.1/tcp/13526",
            "/ip4/192.168.1.103/tcp/13526",
            "/ip4/192.168.100.5/tcp/18625"
         ],
         "Ethereum Address": "0x55B3B672DEB14178615F648911e76b7FE1B23e5D",
         "Peer Type": "provider",
         "Underlay": "16Uiu2HAmBykfyf9A5DnRguHNS1mvSaprzYEkjRf6uafLU4javG4L"
      },
      "connected_peers": {
         "providers": [
            "0xca61596ccef983eb7cae42340ec553dd89881403"
         ]
      }
   }
   ```

6. **Docker Compose Users**:
   - To start the nodes, run:
     ```css
     docker-compose up --build

     ```
   - To stop the service:
     ```

     docker-compose down

     ```

## API Integration

### \***\*For Providers\*\***

Execution providers can use the `provider` role to run their nodes. This allows them to receive signed bids and issue commitments for execution. They will use the **Provider RPC API** to receive signed bids that are being propagated in the network. Once they get a bid, they'll need to communicate with the mev-commit node to **Accept**. Accepted bids result in the mev-commit node sending a signed commitment to the p2p network.

The Provider API is implemented using the gRPC framework, supporting two primary operations:

- **Receiving Bids:** Providers receive streaming bids from the mev-commit node.
- **Sending Processed Bids:** Providers send information about processed bids back to the mev-commit node.

### **RPC API**

Users can find the protobuf file in the [repository(opens in a new tab)](https://github.com/primevprotocol/mev-commit/blob/main/rpc/providerapi/v1/providerapi.proto). This can be used to generate the client for the RPC in the language of your choice. The Go client has already been generated in the repository. For other languages, please follow the instructions in the [gRPC documentation(opens in a new tab)](https://grpc.io/docs/languages/) to generate them separately.

There are two main APIs:

```
  // ReceiveBids is called by the execution provider to receive bids from the mev-commit node.  // The mev-commit node will stream bids to the execution provider.   rpc ReceiveBids(EmptyMessage) returns (stream Bid) {}   // SendProcessedBids is called by the provider to send processed bids to the mev-commit node.  // The execution provider will stream processed bids to the mev-commit node.   rpc SendProcessedBids(stream BidResponse) returns (EmptyMessage) {}
```

The message definitions are as follows:

```
message Bid {  string txn_hash = 1;  int64 bid_amt = 2;  int64 block_number = 3;  bytes bid_hash = 4;}; message BidResponse {  bytes bid_hash = 1;  enum Status {    STATUS_UNSPECIFIED = 0;    STATUS_ACCEPTED = 1;    STATUS_REJECTED = 2;  }  Status status = 2;};
```

### **HTTP API**

The same API is also available on the HTTP port configured on the node. Please review the [API docs(opens in a new tab)](https://mev-commit-docs.s3.amazonaws.com/provider.html) to understand the usage.

An [example client(opens in a new tab)](https://github.com/primevprotocol/mev-commit/tree/main/examples/provideremulator) is implemented in the repository. This example showcases how to integrate a client into the provider's operational framework. While the sample client automatically accepts every incoming bid, providers should incorporate their own decision-making logic to evaluate and respond to these bids.

### F\***\*or Bidders\*\***

Bidders will use the `bidder` role to run their nodes. The node provides the **Bidder API** to submit bids to the network and will sign the bid before sending it out. In response, bidders will receive commitments from providers if their bid is accepted. This is a streaming response, and bidders are expected to keep their connection alive until their node receives all relevant commitments.

The Bidder API is also implemented using the gRPC framework, supporting two primary operations:

- **Send Bid:** User submit their bid.
- **Receive Pre-Confirmation: The u**ser receives streaming pre-confirmations if accepted.

### **RPC API**

Users can find the protobuf file in the [repository(opens in a new tab)](https://github.com/primevprotocol/mev-commit/blob/main/rpc/userapi/v1/userapi.proto). This can be used to generate the client for the RPC in the language of your choice. The Go client has already been generated in the repository. For other languages, please follow the instructions in the [grpc documentation(opens in a new tab)](https://grpc.io/docs/languages/) to generate them separately.

The API available is:

```
  rpc SendBid(Bid) returns (stream PreConfirmation)
```

The message definitions are as follows:

```
message Bid {  string tx_hash = 1;  int64 amount = 2;  int64 block_number = 3;}; message PreConfirmation {  string tx_hash = 1;  int64 amount = 2;  int64 block_number = 3;  string bid_digest = 4;  string bid_signature = 5;  string pre_confirmation_digest = 6;  string pre_confirmation_signature = 7;};
```

### **HTTP API**

The same API is also available on the HTTP port configured on the node. Please review the [API docs(opens in a new tab)](https://mev-commit-docs.s3.amazonaws.com/user.html) to understand the usage.

An [example CLI application(opens in a new tab)](https://github.com/primevprotocol/mev-commit/tree/main/examples/usercli) is implemented in the repository. The primary purpose of this example is to demonstrate the process of integrating with the RPC API.

## Debugging

### **Debugging Tools in mev-commit Node**

As the mev-commit node is under active development, it includes debugging tools for in-depth analysis. These tools, primarily intended for developers and not for everyday users, offer insights into the node's functionality. They are accessible via the HTTP API endpoint.

### **Understanding Network Topology**

You can view the network's topology as recognized by the mev-commit node at the `/topology` endpoint. This provides information such as the number of connected nodes and their roles, helping to ensure your node is well-connected within the network.

Example of Topology Data:

```
{  "self": {    "Addresses": [      "/ip4/127.0.0.1/tcp/13522",      "/ip4/172.29.18.2/tcp/13522"    ],    "Ethereum Address": "0xB8c29FfD307067471fd7997873DfC496Fd02956f",    "Peer Type": "bootnode",    "Underlay": "16Uiu2HAmLYUvthfDCewNMdfPhrVefBbsfaPL22fWWfC2zuoh5SpV"  },  "connected_peers": {    "bidders": [      "0xb9286cb4782e43a202bfd426abb72c8cb34f886c"    ]  },  "blocked_peers": [    {      "Peer": "0x48ddc642514370bdafad81c91e23759b0302c915",      "Reason": "insufficient stake",      "Duration": "1m58.244219375s"    }  ]}
```

### **Prometheus Metrics for Monitoring**

The node emits various Prometheus metrics, which can be utilized to create dashboards displaying the node's statistics. These include default metrics from the `libp2p` networking library and are available at the `/metrics` endpoint.

### **Pprof Endpoints**

For advanced users, pprof endpoints are available at `/debug/pprof` for performance profiling, such as monitoring memory and CPU usage. However, understanding and using these endpoints requires prior knowledge of pprof tools and is not covered in this documentation.

# Contracts

The purpose of smart contracts is to verify and manage the movement of funds as a response to bid and commitment activities that occur on our p2p network.

![Contract Architecture.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Contract_Architecture.png)

## Contract Architecture and Functionality

The Core Protocol has 4 Key Contracts:

- PreConfirmations Contract
  - This will persist details about the commitments that have been made into contract storage
- Bidder Registry Contract
  - This will be where the bulk of monetary transfer occurs
  - Bidders prepay for bids into this contract
  - The payment gets processed into a provider-allocated section that a provider can later retrieve
- Provider Registry Contract
  - This contract stores the stake a provider bounds to ensure it doesn’t break its promises under pre-confirmations
- Oracle Contract
  - This simply retrieves details on which commitments to process for rewarding or slashing.
  - Access to the functions on this contract is restricted to the owner of the Oracle key

## Whitelist Contract

We envision mev-commit to eventually integrate a multitude of bridges to other chains, using multiple tech stacks. Think wormhole, layerzero, etc. Only whitelisted (approved/active) bridging contracts should be able to mint/burn native ether. Thus the following intermediary whitelist contract is proposed.

![Untitled](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Untitled%202.png)

A whitelist contract is responsible for managing a whitelist of addresses that can mint/burn native ether as a part of bridging. Note this contract can be easily integrated with any bridging implementation. Most of the time, integration is simple - replace a line of code that mints wrapped ERC20 tokens on the destination chain with a call to the whitelist contract’s mint function.

To demonstrate how this contract can be integrated with a bridge, we use hyperlane as an example. First, the address where the whitelist contract will be deployed is precomputed with the CREATE2 opcode as a function of the deployer address, salt, and contract bytecode. This precomputed address is hardcoded or set into both the custom geth clients’ precompiles, and the hyperlane contracts.

Next, the mev-commit chain is started from genesis, and the hyperlane bridging contracts are deployed. The predetermined admin account then deploys the whitelist contract, and finally submits a permissioned transaction that adds the `HypERC20.sol` contract address to the whitelist. Note only the admin account is able to mutate the on-chain whitelist, and will mutate this list for existing bridge upgrades, or new bridge deployments.

Note the `HypERC20.sol` contract inherits ERC20Upgradeable, but the hyperlane stack doesn’t seem to implement a proxy contract. Luckily the whitelist contract naturally makes `HypERC20.sol` upgradable in that the whitelist is mutable.

## Fee Vault Contract

The mev-commit chain implements a fee mechanism where all base fees accumulate to a "fee vault" contract. The predetermined address for such a contract is hardcoded into mev-commit-geth.

The fee vault contract is initiated with an immutable treasury address. The contract owner can invoke transfers of the accumulated fees from the sidechain contract, to the treasury address on L1, including bridging.

Since the treasury address is immutable, the contract owner only has the power to initiate transfers. The separation between the contract owner account and treasury account is useful in that key management can be separate. Ie. a secure multisig governing the protocol treasury doesn't need to submit regular transfer transactions.

This idea is WIP → https://github.com/primevprotocol/contracts/pull/66

## Deploying Custom Contracts

You can deploy your own custom contracts on our mev-commit chain. To do so, simply set your **Chain ID to 17864** and RPC Url to XYZ. Now you can deploy any EVM compatible contracts to the mev-commit chain. Feel free to use Foundry, Hardhat, or any other smart-contract development and deployment framework.

# Bridging and Fees

### **Bridged ether**

Native ether on the mev-commit chain maintains a 1:1 peg with ether on L1. That is, the only way to mint ether on the mev-commit chain is to lock equivalent ether in the hyperlane bridge contract on L1. In the other direction, ether can be burned on the mev-commit chain to unlock equivalent ether from the L1 contract.

Ether that is used as gas on the mev-commit chain will accumulate in a Primev owned treasury account on L1.

There are inherent security assumptions in obtaining ether on the mev-commit chain, involving:

- Hyperlane validators that attest to merkle inclusion of state.
- POA signers that maintain mev-commit chain state.
- The hyperlane bridging protocol and integration into the mev-commit chain.

The Primev core team is committed to secure and live bridging, all system components can be permissionlessly validated by anyone.

## Understanding Bridging Mechanisms

Largely addressed in [mev-commit chain bridge design and failsafes (WIP)](https://www.notion.so/mev-commit-chain-bridge-design-and-failsafes-WIP-c4dfa6e3bea845e79dd9fdcaa5980c52?pvs=21). Most important sections are “High Level Components” and “Bridge Protocol Customizations”

## Fee Structures and Transactions

Let’s consider the relationship between mev-commit native ether, and aggregate mainnet L1 ether that is locked in bridging contracts. The mev-commit chain fee mechanism must be non-inflationary. Otherwise, overtime there’d be more ether on our sidechain than could be withdrawn from the L1 bridge contract.

On mainnet ethereum with EIP-1559, proposers receive a block reward created by the network, along with any accumulated priority fees for transactions in the block. Base fees are burned. Depending on network load, this fee mechanism can be inflationary or deflationary.

The mev-commit chain will implement a fee model that maintains a 1:1 peg between sidechain ether and L1 escrowed ether, meaning **any transaction executed on the mev-commit does not change the total supply of native ether**. This property circumvents the need for accounting logic related to bridging.

Specifically, the mev-commit chain implements a variation of EIP-1559. The clique POA protocol does [not have block rewards](https://github.com/primevprotocol/go-ethereum/blob/7d52a7068edd08650dc267d26cb8059ece55bc84/consensus/clique/clique.go#L587). Base fees accumulate to a Primev treasury address, and priority fee sent to the signer of a block. See [this PR](https://github.com/primevprotocol/go-ethereum/pull/9). Separate application level fees can be defined as necessary.

# Security Best Practices

## Failsafe Mechanisms

Even with great op-sec and the protocols described above, it’ll be desirable to have worst case scenario fail safe mechanisms set in place for the bridge. L1 Ethereum is naturally the “canonical chain”, and failsafes should revolve around the idea that correct L1 state is most important.

Note some of these mechanisms may already exist “off the shelf” for competitors to hyperlane. Hyperlane provides a lot of flexibility, at the cost of having to build these security mechanisms ourselves. It’s not feasible to implement everything described here before testnet release. However, this section can at least serve as a useful wishlist for any bridging protocol we decide to integrate in the medium/long term.

### mev-commit chain halt

Implementation Status: N/A ✅

In the scenario that mev-commit chain state is compromised, the easiest failsafe is to halt the mev-commit chain and/or bridge validators before any auxiliary damage can be done. For example, the validator set monitoring L1 state could be compromised, or the deployer admin multisig mentioned above could be compromised.

### L1 Unlock Rate Limiting

Implementation Status: In progress (externally) ⚒️

Since anyone can unlock and receive funds from the L1 hyperlane contract with proper message delivery and validation, rate limiting is extremely important. If mev-commit chain state and/or the bridge validator set (monitoring mev-commit chain state) are compromised, an attacker could only steal a rate limited amount of ether on L1 over time. This allows time for developers to take action during an attack scenario, before the L1 contract is drained of funds. This rate limiting mechanism would be implemented within the hyperlane ISM that’ll exist on L1.

Solidity has support for accessing the current block’s `block.number` or `block.timestamp` allowing us to define custom logic as to how much ether can be unlocked to users per time period. A simple protocol could specify that `X` ether can be unlocked from L1 every `Y` seconds, where we tune the `X` and `Y` parameters as desired. If the unlock limit is reached, an event is emitted which can alert a monitoring service that a potential attack has ensued. At that time the core team can either leave the system alone if the unlock is legit, or freeze the bridge.

This protocol seems simple, but would require a “meter” variable tracking ether unlocked during the current time period. As well as logic around resetting said meter variable once the current time period has elapsed. Since EVM does not permit automatic contract execution, we’d also have to integrate retry logic for bridge users. See [a similar protocol](https://cosmos.github.io/interchain-security/adrs/adr-002-throttle) designed for the cosmos hub.

It’s likely that some of the available bridging projects have rate limiting mechanisms that we can use. Hyperlane has an [open issue for it](https://github.com/hyperlane-xyz/hyperlane-monorepo/issues/2697).

In the future it’ll be desirable to rate limit minting/burning of native ether on a per-bridge basis. This could depend on the particular bridge implementation in question, or we could build our own generalizable rate limiter.

### Bridge Recovery Mode Ideas

Implementation Status: Not started ❌

Hyperlane allows for customization around security failsafes. We can leverage a [Routing ISM](https://v2.hyperlane.xyz/docs/protocol/sovereign-consensus/routing-ism) to toggle between one of two security models on L1. In the normal scenario, a hyperlane validator multisig is used for message validation.

In an emergency scenario, a more secure multisig (likely the same as Primev treasury multisig) sends a permissioned tx to the Routing ISM, which freezes normal bridge unlocks. The normal hyperlane validator multisig is no longer valid, and bridge control is delegated to the more secure treasury multisig.

From here, we have some options:

- Interchain messages could be validated manually by the more secure multisig
- Time locks could be used to restore normal functionality to the bridge after a week or two

Clearly there’s lots of options here, each with tradeoffs, but unfortunately not a lot of already built solutions for the hyperlane stack. Building and testing these ideas ourself is feasible, but likely a multi week task.

### Upgrade Contracts

Implementation Status: Not started ❌

Hyperlane’s [Mailbox.sol](https://github.com/primevprotocol/hyperlane-monorepo/blob/69850642365251e16b9f23b87d6caf187036b3ee/solidity/contracts/Mailbox.sol) inherits openzepellin’s `OwnableUpgradeable` which can be used to upgrade the contract in an emergency scenario. See docs on contract upgrading [here](https://docs.openzeppelin.com/learn/upgrading-smart-contracts). This failsafe requires further research and will come with intricacies.

# Join Testnet with Node

To join the mev-commit chain testnet with your own full-node, use [mev-commit geth (opens in a new tab)](https://github.com/primevprotocol/go-ethereum). We've modified go-ethereum to achieve shorter block periods than mainnet Ethereum, and to enable seamless native token bridging capabilities. Geth configuration will vary based on environment, but an example is provided below:

```
exec geth \    --verbosity="$VERBOSITY" \    --datadir="$GETH_DATA_DIR" \    --port 30311 \    --syncmode=full \    --gcmode=full \    --http \    --http.corsdomain="*" \    --http.vhosts="*" \    --http.addr=0.0.0.0 \    --http.port="$RPC_PORT" \    --http.api=web3,debug,eth,txpool,net,engine \    --bootnodes enode://34a2a388ad31ca37f127bb9ffe93758ee711c5c2277dff6aff2e359bcf2c9509ea55034196788dbd59ed70861f523c1c03d54f1eabb2b4a5c1c129d966fe1e65@172.13.0.100:30301 \    --networkid=$CHAIN_ID \    --unlock=$BLOCK_SIGNER_ADDRESS \    --password="$GETH_DATA_DIR"/password \    --mine \    --miner.etherbase=$BLOCK_SIGNER_ADDRESS \    --allow-insecure-unlock \    --nousb \    --netrestrict 172.13.0.0/24 \    --metrics \    --metrics.addr=0.0.0.0 \    --metrics.port=6060 \    --ws \    --ws.addr=0.0.0.0 \    --ws.port="$WS_PORT" \    --ws.origins="*" \    --ws.api=debug,eth,txpool,net,engine \    --rpc.allow-unprotected-txs \    --authrpc.addr="0.0.0.0" \    --authrpc.port="8551" \    --authrpc.vhosts="*" \    --nat extip:$NODE_IP
```

Note this configuration will be productionized further in the coming weeks.

# Fees and Funds Movement

Funds are moved in the following scenarios:

1. Allowance/prepayment by bidder.
2. Staking by provider.
3. Rewarding provider with pre-confirmation from bidder.
4. Slashing for unfulfilled pre-confirmation promises.

## Fee Structure of a Bid

During the deployment of the Bidder Registry, a protocol-specific Fee Percentage is set (see [here](https://github.com/primevprotocol/contracts/blob/main/contracts/BidderRegistry.sol#L79)). This percentage of the Bid's value is allocated to the protocol Treasury.

![Bidder Alloawnce.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Bidder_Alloawnce.png)

In general, for deployment to Testnet, the current specified fee percentage is 2%.

## Slashing Fee Structure

When a provider fails to fulfill a promise, slashing occurs. The slashing amount depends on the bid size. For instance, if a Bidder bids 100,000 Gwei and the provider is to be slashed, the funds flow as described below. This is similar to the Bid Fee Structure, but the funds are drawn from the provider's stake instead of the Bidder.

![Provider Allowance.png](Primev%20Docs%20Testnet%20Ver%2041c24e77a73e47e0966d54365efbbfc8/Provider_Allowance.png)

### Minimum Staking

Providers need to stake a minimum amount in order to Join the p2p Network.

The current minimum requirement is 1 ether set [here](https://github.com/primevprotocol/contracts/blob/main/scripts/DeployScripts.s.sol#L43)

# Bidder Quickstart

## 1 Minute Quickstart

### Pre-requisites

- `curl`
- `terminal`

Simply run the following command in your terminal:

```bash
curl -o launchmevcommit https://raw.githubusercontent.com/primevprotocol/scripts/main/launchmevcommit && chmod +x launchmevcommit && ./launchmevcommit --rpc-url http://69.67.151.95:8545 --node-type bidder
```

## Manual Quickstart

We provide advanced instructions for those of you who want to customize the starting process.

### Pre-requistes

- `curl`
- `terminal`
- `cast` Install using `curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz/) | bash`
  - run foundryup

## Running the Bidder Node

- Step 1. Check your CPU architecture
  ```bash
  uname # This will give you the OS type
  uname -m # This will give you the chip architecture
  ```
- Step 2. Pull The Required Binary of mev-commit based on your CPU architecture
  Download the binaries from the following
  [https://github.com/primevprotocol/mev-commit/releases/tag/v0.1.0](https://github.com/primevprotocol/mev-commit/releases/tag/v0.1.0)
  Unzip the binary and store in a root directory of your choice
- Step 3. Place the binary, named mev-commit, into a folder of your choice
  Note this will be considered the root folder of your mev-commit
- Step 4. Run the initialization
  cd into the folder that contains the mev-commit binary and run the following command
  ```bash
  ./mev-commit init --dir . --rpc-endpoint http://69.67.151.95:8545  --peer-type bidder
  ```
  This will create both a private key file and a config.yaml file and store it in your current directory.
  While in the directory, run the following command to export relevant data about your address and private key:
  ```bash
  export KEY=$(cat key)
  export ADDRESS=$(cast wallet address --private-key 0x$(cat key))
  ```
- Step 5. Fund the account
  In the same shell run the following command to transfer a 100 ether into your wallet from a faucet account:
  ```bash
  cast send --rpc-url [http://69.67.151.95:8545](http://69.67.151.95:8545/) --private-key 0x7c9bf0f015874594d321c1c01ada3166c3509bbd91f76f9e4d7380c2df269c55 $ADDRESS --value 100ether
  ```
- Step 6. Run the node
  1. cd into the folder with your mev-commit binary
  2. Run the Node:
  ```bash
  ./mev-commit start --config=~/config.yaml
  ```
- Step 7. Prepay for bids
  ### Prepaying for Bids
  The bidder needs to prepay a certain amount onto the primev contracts to allow them to send bids and receive commitments. This is to ensure that providers can pull funds once the bids have been completed.
  To prepay, first **********************\*\***********************open a new terminal**********************\*\*********************** and run the following command:
  ```bash
  curl -X POST http://localhost:13523/v1/bidder/prepay/1000000000000000000
  ```
  This will prepay `1000000000000000000 wei` into your account.

## Interacting with the Bidder Node

First you can set the local environment variables to set your KEY and ADDERSS. Change directories to the one with your key (this should be the same folder as mev-commit). If you used the 1 minute quickstart command, your folder would be $HOME/.mevcommitup

Setting these will make running the commands to interact with your account easier.

```bash
export KEY=$(cat key)
export ADDRESS=$(cast wallet address --private-key 0x$(cat key))
```

### Sending Bids

To send bids, we simply run the following command:

```bash
curl -X POST http://localhost:13523/v1/bidder/bid \
-d '{
  "txHash": "91a89B633194c0D86C539A1A5B14DCCacfD47094",
  "amount": <amount in wei>,
  "blockNumber": <blocknumber>
}'
```

<aside>
💡 You can change the values in the fields txHash, amount and blockNumber as desired

</aside>

<aside>
❗ **Ensure you’ve prepayed for bids**

The bidder needs to prepay a certain amount onto the primev contracts to allow them to send bids and receive commitments. This is to ensure that providers can pull funds once the bids have been completed.

To prepay, ensure you’ve got the bidder node up, and run the following command:

```bash
curl -X POST http://localhost:13523/v1/bidder/prepay/1000000000000000000
```

This will prepay `1000000000000000000 wei` into your account.

</aside>

- **********************************\*\***********************************Optional - Use Postman to send bids**********************************\*\***********************************
  If you’ve got a Postman account and postman agent installed, you can use [the following link](https://primev.postman.co/workspace/Team-Workspace~18870d84-94f0-4d1e-8163-db558f83d7e8/request/27192304-fab87a71-9722-46f8-825f-d9791ead6178?ctx=documentation&tab=body) to send bids Via Postman

### Checking Prepayed Balance

To get the current prepayed balance in the contract

```bash
curl -s http://localhost:13523/v1/bidder/get_allowance
```

<aside>
ℹ️ Allowance represents the funds in the bidders account that can be used to submit bids on the primev p2p-network and settled on-chain

</aside>

### Withdraw Prepayed Funds

```bash
cast send 0x62197Abd7672925c7606Bdf9931e42baCa6619AD "withdrawPrepayedAmount(address)" $ADDRESS --rpc-url http://69.67.151.95:8545 --private-key $KEY
```

### Checking Balance of your Wallet

This command will allow you to check your current wallet balance on mev-commit chain

```bash
cast b $ADDRESS --rpc-url http://69.67.151.95:8545
```

### Check Total Value Locked in Contracts

```bash
python3 -c "print($(cast b 0x62197Abd7672925c7606Bdf9931e42baCa6619AD --rpc-url http://69.67.151.95:8545) + $(cast b 0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC --rpc-url http://69.67.151.95:8545))"
```

### Topology

This allows you to view the topology of your machines p2p connections

```bash
curl [http://localhost:13523/topology](http://localhost:13523/topology)
```

## Provider Quickstart

### Pre-requistes

- `curl`
- `terminal`

To become a provider, you need to run the provider node along with a provider emulator or your own gRPC enabled service to interact with the provider node.

To spin up a Provider node, you can simply run the following:

```bash
curl -o launchmevcommit https://raw.githubusercontent.com/primevprotocol/scripts/main/launchmevcommit && chmod +x launchmevcommit && ./launchmevcommit --rpc-url http://69.67.151.95:8545 --node-type provider
```

<aside>
❗ You will need to expose port 13523 on your local machine to allow the provider node to establish connections that are incoming to the node

</aside>

## Manual Quickstart

We provide advanced instructions for those of you who want to customize the starting process.

### Pre-requistes

- `curl`
- `terminal`
- `cast` Install using `curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz/) | bash`
  - run foundryup
- Step 1. Check your CPU architecture
  ```bash
  uname # This will give you the OS type
  uname -m # This will give you the chip architecture
  ```
- Step 2. Pull The Required Binary of mev-commit based on your CPU architecture
  Download the binaries from the following
  [https://github.com/primevprotocol/mev-commit/releases/tag/v0.1.0](https://github.com/primevprotocol/mev-commit/releases/tag/v0.1.0)
  Unzip the binary and store in a root directory of your choice
- Step 3. Place the binary, named mev-commit, into a folder of your choice
  Note this will be considered the root folder of your mev-commit
- Step 4. Run the initialization
  cd into the folder that contains the mev-commit binary and run the following command
  ```bash
  ./mev-commit init --dir . --rpc-endpoint http://69.67.151.95:8545  --peer-type provider
  ```
  This will create both a private key file and a config.yaml file and store it in your current directory.
  While in the directory, run the following command to export relevant data about your address and private key:
  ```bash
  export KEY=$(cat key)
  export ADDRESS=$(cast wallet address --private-key 0x$(cat key))
  ```
- Step 5. Fund the account
  cd to the folder where you’ve stored the key and run the following command to transfer a 100 ether into your wallet from a faucet account:
    <aside>
    ❗ If you’ve named the private key file something other than key, change key to the correct name in `--private-key 0x$(cat ***key***))`
    
    </aside>
    
    ```bash
    cast send --rpc-url [http://69.67.151.95:8545](http://69.67.151.95:8545/) --private-key 0x7c9bf0f015874594d321c1c01ada3166c3509bbd91f76f9e4d7380c2df269c55 $(cast wallet address --private-key 0x$(cat key)) --value 100ether
    ```

- Step 6. Run the node
  1. cd into the folder with your mev-commit binary
  2. Run the Node:
  ```bash
  ./mev-commit start --config=~/config.yaml
  ```
- Step 7. Stake Funds and Register
  This command is used to ensure that your provider has been registered with a stake in our protocol. This is used to ensure it can connect with the mev-commit-p2p network. Ensure you run this command in the mev-commit directory.
  ```bash
  cast send 0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC "registerAndStake()" $(cast wallet address --private-key 0x$(cat key)) --rpc-url http://69.67.151.95:8545 --private-key $(cat key) --value 100ether
  ```

## gRPC Node Connection

Once a provider node is up and running, you can use the `ReceiveBids` stream to receive bids into offline infrastructure and send decisions based on those bids via the `SendProcessedBids` stream.

<aside>
❗ **By default, this service is disabled** and must be enabled by setting the ProviderAPIEnabled flag in the config file to true.

</aside>

- Config file with ProviderAPIEnabled set to true
  ```yaml
  **expose_provider_api: true**
  priv_key_file: /path/to/key
  peer_type: provider
  p2p_port: 13522
  http_port: 13523
  rpc_port: 13524
  secret: hello
  log_fmt: text
  log_level: debug
  bidder_registry_contract: 0x62197Abd7672925c7606Bdf9931e42baCa6619AD
  provider_registry_contract: 0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC
  preconf_contract: 0xBB632720f817792578060F176694D8f7230229d9
  rpc_endpoint: http://69.67.151.95:8545
  bootnodes:
    - /ip4/69.67.151.95/tcp/13522/p2p/16Uiu2HAmLYUvthfDCewNMdfPhrVefBbsfaPL22fWWfC2zuoh5SpV
  ```
- gRPC API Specification
  ```bash
  // ReceiveBids is called by the execution provider to receive bids from the mev-commit node.
  // The mev-commit node will stream bids to the execution provider.
  rpc ReceiveBids(EmptyMessage) returns (stream Bid) {}
  // SendProcessedBids is called by the provider to send processed bids to the mev-commit node.
  // The execution provider will stream processed bids to the mev-commit node.
  rpc SendProcessedBids(stream BidResponse) returns (EmptyMessage) {}
  ```

[The following file](https://github.com/primevprotocol/mev-commit/blob/main/integrationtest/provider/client.go) shows an implementation of a wrapped client that interfaces with the GRPC API client that was autogenerated [here](https://github.com/primevprotocol/mev-commit/blob/main/gen/go/rpc/providerapi/v1/providerapi.pb.go).

# Github Link

# Tutorials

## Receiving your First Pre-Confirmation

We have a running mev-commit provider node on our testnet that will always return you a Pre-Confirmation. However, this provider is not a builder, and will not produce blocks. The node is purely to showcase the process of submitting a bid and waiting for a pre-confirmation from the network.

### Step 1. Get the Bidder Node up and running

```bash
curl -o launchmevcommit https://raw.githubusercontent.com/primevprotocol/scripts/main/launchmevcommit && chmod +x launchmevcommit && ./launchmevcommit --node-type bidder
```

This will fund your wallet, and start a new mev-commit bidder node for you.

If you’ve already run the command, it will ask you if you want to overwrite, for a clean setup, respond with a `y`.

### Step 2. Submit Bid and wait for Pre-Confirmation

Change the `txHash` `amount` and `blockNumber` to your liking

```bash
curl -X POST http://localhost:13523/v1/bidder/bid \
-d '{
  "txHash": "91a89B633194c0D86C539A1A5B14DCCacfD47094",
  "amount": 20000,
  "blockNumber": 899099
}'
```

🎉 Congrats, you’ve should receive a Pre-Confirmation!

# **Differences between Ethereum and mev-commit chain**

The most significant difference between Ethereum and mev-commit-chain lies in their block times.

| Ethereum Blocktime | mev-commit-chain Blocktime |
| ------------------ | -------------------------- |
| 12 seconds         | 200 milliseconds           |

### Other Chain Considerations:

1. **Transaction Fees:** Similar to Ethereum, supporting both legacy and EIP-1559 transactions. EIP-1559 preferred.
2. **Mempool:** mev-commit-chain's mempool is smaller than Ethereum's. Public, but only primev runs nodes, thus controlling mempool visibility.
3. **Network Size:** Small network with 3 nodes on mev-commit-chain, leading to a smaller mempool.
4. **Opcodes:** All Ethereum opcodes are supported on mev-commit chain, as it consists of Ethereum nodes.
5. **Consensus**: Although mainnet Ethereum uses proof of stake, mev-commit-chain uses clique proof of authority
