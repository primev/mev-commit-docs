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

# Differences Between Ethereum and mev-commit Chain

### Block times

The most significant difference between Ethereum and mev-commit-chain lies in their block times.

| Ethereum Blocktime | mev-commit-chain Blocktime |
| ------------------ | -------------------------- |
| 12 seconds         | 200 milliseconds           |

### **Transaction Fees**

Like Ethereum, mev-commit-chain supports both legacy and EIP-1559 transactions, with a preference for EIP-1559.

The allocation of fees differs:

* The base fee is directed to the protocol treasury, instead of being burned.
* The priority fee goes to the proposer/signer, as in Ethereum.

### Other Chain Considerations:

1. **Mempool:** mev-commit-chain's mempool is smaller than Ethereum's. It's public; however, as primev is the currently sole node operator, this restricts visibility of the mempool.
2. **Number of validators:** the mev-commit chain has only two nodes creating blocks in a round-robin fashion.
3. **Opcodes:** All Ethereum opcodes are supported on mev-commit chain, as it consists of Ethereum nodes.
4. **Consensus**: Although mainnet Ethereum uses proof of stake, mev-commit-chain uses clique proof of authority
