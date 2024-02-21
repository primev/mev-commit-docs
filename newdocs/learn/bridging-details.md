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

# Bridging Details

## Understanding Bridging Mechanisms

The mev-commit chain utilizes multiple mechanisms to ensure secure and timely transfer of assets between Ethereum mainnet (L1) and the mev-commit chain. Here's a detailed breakdown:

### **Lock/Mint Protocol**

* **Implementation**: A bidirectional lock/mint bridge exists between Ethereum's Holesky testnet and the mev-commit chain. The bridge's implementation is maintained in the [mev-commit-bridge repo](https://github.com/primevprotocol/mev-commit-bridge/tree/main/standard).
* **Transfer Flow**: To bridge, a user sends a `TransferInitiated` tx  with associated metadata, which locks an amount of ether on the source chain. Once this transaction is included, an off-chain relayer agent listens for the corresponding event and submits a `TransferFinalized` tx on the destination chain. The transfer is complete upon the inclusion of the `TransferFinalized` tx on the destination chain, where funds are released to the specified account.

### **Gateway Contracts**

* **Native Ether Only**: The mev-commit chain only supports native ether. That is, ERC20 tokens are not bridgeable to the mev-commit chain, nor are they usable as gas.
* **Management of Liquidity**: a _bridge gateway_ contract exists on both L1 and the mev-commit chain, acting as entrypoints for the lock/mint protocol. The [L1Gateway](https://github.com/primevprotocol/contracts/blob/main/contracts/standard-bridge/L1Gateway.sol) serves as an escrow contract by which ether is locked for transfers to the mev-commit chain, and unlocked for transfers back to L1. The [SettlementGateway](https://github.com/primevprotocol/contracts/blob/main/contracts/standard-bridge/SettlementGateway.sol) simply mints or burns native ether on the mev-commit chain, via the whitelist contract interface.

### **Whitelist Contract**

* **Managing Bridging Permissions**: A whitelist contract deployed to the mev-commit chain manages addresses authorized to mint or burn native ether as part of the bridging process. This contract is crucial in maintaining control over which bridging contracts are "active". We envision multiple bridges being integrated with the mev-commit chain in the future.

### **Importance of Origin Chain Security**

* **Selective Bridging**: Only bridges originating from Ethereum L1 should be allowed to mint native ether on the mev-commit chain. This is to prevent the impact of compromised states on other chains affecting the mev-commit chain.

### **Fee Mechanism**

* **Non-inflationary Approach**: The fee mechanism on the mev-commit chain is designed to be non-inflationary, maintaining a strict 1:1 peg between usable sidechain ether and ether escrowed in the L1 gateway contract. This is achieved through a variation of EIP-1559, where base fees are accumulated in a treasury and priority fees are allocated to block signers.

### Learn More

To learn more about the bridge between Holesky and the mev-commit chain, refer [here](https://github.com/primevprotocol/mev-commit-bridge/blob/main/standard/bridge-v1/README.md) and explore the implementation details.
