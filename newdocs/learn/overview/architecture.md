---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Architecture

<figure><img src="../../.gitbook/assets/System_Overview_-_b2.png" alt=""><figcaption></figcaption></figure>

The system architecture of mev-commit is composed of three primary components:

* **API**:
  * Features a gRPC/HTTP interface for main API interactions.
  * Includes debug endpoints to access metrics, pprof, and other relevant information.
* **P2P Node**:
  * Operates as a libp2p node, utilizing custom protocols for network communication.
* **Settlement Client**:
  * Functions as a wrapped Ethereum client, responsible for issuing and managing transactions.

## mev-commit P2P Node

The mev-commit P2P network comprises three distinct types of nodes:

* **Bootnodes** are nodes managed by the Primev team that provides service discovery. Bootnodes only run a subset of the P2P protocols that are responsible for creating connections and gossiping peer information across the network to achieve full connectivity between the different nodes.
* **Bidder nodes** create signed bids and broadcast them to all the provider nodes on the mev-commit network and receive the signed commitments from the providers. Providers can accept/reject bids. The bidders will get commitments only for the accepted bids.
* **Provider nodes** will receive the signed bids from the bidders. They will communicate with the provider block-building infrastructure and decide whether they will accept the bid or not. Providers will accept bids if they have decided to include the transaction in the block that they are planning to propose. The commitments are also sent to the mev-commit settlement chain where they will be settled once the block is built on the L1 chain.

### **Services Offered by mev-commit Nodes**

In addition to the core architecture components, mev-commit nodes offer a range of services essential for the network's functionality. These services are designed to facilitate various interactions within the mev-commit ecosystem, ranging from API communications to peer-to-peer networking. Below are the key services that each mev-commit node starts:

* **RPC Service**: Operating on the default port 13524, this service provides a gRPC API interface, enabling interactions with the provider and bidder APIs.
* **HTTP Service**: Available on the default port 13523, it offers a REST API for interactions similar to the RPC service, along with additional debugging APIs.
* **P2P Service**: Functioning on the default port 13522, this service is crucial for peer-to-peer communications within the mev-commit network, utilizing various custom protocols.

### **Core Protocols in P2P Node**

The P2P node implements key protocols to facilitate interaction among different actors. Currently, there are 3 main protocols:

*   **Handshake:** When the nodes initially connect, they exchange signed messages with each other to authenticate themselves. They are allowed to participate in the other protocols only after the successful conclusion of the handshake.

    * Provider nodes need to be **staked** in the provider registry before they can connect to other nodes.

    <figure><img src="../../.gitbook/assets/Handshake.png" alt=""><figcaption></figcaption></figure>
* **Discovery:** To establish connections with other actors, the nodes need to gossip underlay information. The discovery protocol is used to achieve this. Currently, the network tries to establish a mesh topology where all the nodes are connected.

<figure><img src="../../.gitbook/assets/discovery (2).png" alt=""><figcaption></figcaption></figure>

* **Preconfirmations:** Once the nodes establish connectivity, they will start interacting with other actors on the preconfirmations protocol to exchange signed bids and commitments.

<figure><img src="../../.gitbook/assets/preconfirmations.png" alt=""><figcaption></figcaption></figure>
