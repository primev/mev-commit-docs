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

# Provider Attribution Methodology

The current methodology for identifying the provider (also known as the builder) responsible for constructing the L1 block relies on the **`extra_data`** field within the block's execution payload. We correlate this information with the corresponding address registered in the mev-commit protocol. There are ongoing efforts to develop and implement more cryptographically secure methods of attribution.

## Oracle Roadmap

In the longer term, we plan to enable stronger provider attribution through the use of shortened signatures in the extra-data payload.

## Slashing Conditions

**Unsatisfied promise**: In the condition where a provider was meant to carry out a promise, and was able to carry out the promise (e.g was able to deliver payload into target slot), but refused to do so.

### **Bridge to Sepolia**

The mev-commit chain is bridged to Sepolia via a [hyperlane warp route(opens in a new tab)](https://docs.hyperlane.xyz/docs/protocol/warp-routes), involving multiple agents delivering or validating cross-chain messages.

Users initiate a bridge transaction on L1 by locking ether in the bridge contract. A permissioned validator sets monitors for these transactions and attests to their merkle inclusion on L1. A permissionless relayer agent then delivers the message to the mev-commit chain’s `Interchain Security Module` (ISM), which verifies validator signatures and mints native ether on the mev-commit chain as needed.

Bridging ether from the mev-commit chain back to L1 follows a similar process, except that native ether is burned on the mev-commit chain, and unlocked on L1.

Hyperlane exposes a contract interface that allows bridge users to pay ether to cover the costs of delivering a message on the destination chain. See [Interchain Gas Payment](https://docs.hyperlane.xyz/docs/protocol/interchain-gas-payment) for more details.

Running a hyperlane relayer is permissionless, and we encourage anyone to run their own relayer relevant to the mev-commit chain bridge. See [Running Relayers](https://docs.hyperlane.xyz/docs/operate/relayer/run-relayer) for more details.
