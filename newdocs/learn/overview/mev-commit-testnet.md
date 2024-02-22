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

# mev-commit Testnet

## Introduction to Testnet and its Purpose

The testnet allows bidders and providers to spin up nodes and send bids and commitments along. This will allow bidders to get pre-confirmations from providers on the Holesky network.

## **mev-commit chain connection details**

### **Services links**

| Service           | Link                                                       |
| ----------------- | ---------------------------------------------------------- |
| Bootnode endpoint | [http://34.215.163.180:8545/](http://34.215.163.180:8545/) |
| Bridge frontend   | `Coming soon`                                              |
| Block explorer    | [http://34.209.10.199/blocks](http://34.209.10.199/blocks) |

### **Contract addresses**

<table><thead><tr><th width="299">Contract</th><th>Address</th></tr></thead><tbody><tr><td>UserRegistry</td><td>0x390066a15e1048445F1B1b69Ba98AC4cb5e91c52</td></tr><tr><td>ProviderRegistry</td><td>0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC</td></tr><tr><td>PreConfCommitmentStore</td><td>0xBB632720f817792578060F176694D8f7230229d9</td></tr><tr><td>Oracle</td><td>0x51dcB14bcb0B4eE747BE445550A4Fb53ecd31617</td></tr><tr><td>Whitelist</td><td>0xc5bB85F941fb8dbbed6416A8aC84A06226E0f138</td></tr></tbody></table>



## Infrastructure

Primev will maintain a testnet consisting of the mev-commit chain, oracle service, and bridge to Holesky.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## Differences between the testnet and mainnet environments

Testnet will use Holesky and Holesky eth, whereas mainnet uses mainnet eth. Commitments are also written to the settlement in plaintext, whereas mainnnet will include private commitments.

## Limitations of the testnet

We do not yet support a decay mechanism that would allow for higher rewards to accrue to a provider when they submit a commitment earlier into a slot.

We also do not have bid and commitment privacy beyond the encrypted channels that exist in the p2p communication across the network. This means that when a commitment and bid are stored on the chain, they’re public.

## How users can provide feedback or report issues encountered on the testnet?

Users can open an issue on the GitHub repository that is associated with any issue they encounter. They can also tag @primev\_xyz on Twitter.

## Testnet System Requirements

Please refer to System Requirements section [here.](https://www.notion.so/Primev-Docs-Testnet-Ver-41c24e77a73e47e0966d54365efbbfc8?pvs=21)

