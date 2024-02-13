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

# How It Works

## How does mev-commit work?

At its core, mev-commit establishes a dynamic and efficient conduit for MEV actors, such as searchers and block builders, to exchange execution bids and commitments. The process begins with MEV actors joining the mev-commit network, where they can broadcast and receive bids in real-time. These bids, encapsulating transaction details and proposed execution fees, are communicated across the network. Upon receipt, execution providers evaluate these bids and may issue binding commitments if they choose to include the transactions in an upcoming block. This mechanism not only ensures a transparent and competitive environment for MEV extraction but also contributes to a more stable and equitable blockchain ecosystem by prioritizing clarity and fairness in transaction execution.

Mev-commit's p2p network is structured to allow real-time communication across network actors:

<figure><img src="../../.gitbook/assets/Untitled 1.png" alt=""><figcaption></figcaption></figure>

**Connections:** Users and providers are interconnected, with each node initially accessing a primary network node (bootnode) for startup.

**Gateway Nodes:** Providers can set up gateway nodes, allowing bid submissions to their mev-commit RPC endpoint first. These bids will also be gossiped amongst providers.

The diagram below illustrates how **bids, commitments, and funds** flow with mev-commit, reflecting its efficiency in facilitating P2P interactions for mev actors.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
