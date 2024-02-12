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

# Contracts

The purpose of smart contracts is to verify and manage the movement of funds as a response to bid and commitment activities that occur on our p2p network.

<figure><img src="../../.gitbook/assets/Contract_Architecture.png" alt=""><figcaption></figcaption></figure>

## Contract Architecture and Functionality

The Core Protocol has 4 Key Contracts:

* **PreConfirmations Contract**
  * This will persist details about the commitments that have been made into contract storage
* **Bidder Registry Contract**
  * This will be where the bulk of monetary transfer occurs
  * Bidders prepay for bids into this contract
  * The payment gets processed into a provider-allocated section that a provider can later retrieve
* **Provider Registry Contract**
  * This contract stores the stake a provider bounds to ensure it doesn’t break its promises under pre-confirmations
* **Oracle Contract**
  * This simply retrieves details on which commitments to process for rewarding or slashing.
  * Access to the functions on this contract is restricted to the owner of the Oracle key

## Whitelist Contract

We envision mev-commit to eventually integrate a multitude of bridges to other chains, using multiple tech stacks. Think wormhole, layerzero, etc. Only whitelisted (approved/active) bridging contracts should be able to mint/burn native ether. Thus the following intermediary whitelist contract is proposed.

<figure><img src="../../.gitbook/assets/Untitled 2.png" alt=""><figcaption></figcaption></figure>

A whitelist contract is responsible for managing a whitelist of addresses that can mint/burn native ether as a part of bridging. Note this contract can be easily integrated with any bridging implementation. Most of the time, integration is simple - replace a line of code that mints wrapped ERC20 tokens on the destination chain with a call to the whitelist contract’s mint function.

To demonstrate how this contract can be integrated with a bridge, we use hyperlane as an example. First, the address where the whitelist contract will be deployed is precomputed with the CREATE2 opcode as a function of the deployer address, salt, and contract bytecode. This precomputed address is hardcoded or set into both the custom geth clients’ precompiles, and the hyperlane contracts.

Next, the mev-commit chain is started from genesis, and the hyperlane bridging contracts are deployed. The predetermined admin account then deploys the whitelist contract, and finally submits a permissioned transaction that adds the `HypERC20.sol` contract address to the whitelist. Note only the admin account is able to mutate the on-chain whitelist, and will mutate this list for existing bridge upgrades, or new bridge deployments.

Note the `HypERC20.sol` contract inherits ERC20Upgradeable, but the hyperlane stack doesn’t seem to implement a proxy contract. Luckily the whitelist contract naturally makes `HypERC20.sol` upgradable in that the whitelist is mutable.

## Fee Vault Contract

The mev-commit chain implements a fee mechanism where all base fees accumulate to a "fee vault" contract. The predetermined address for such a contract is hardcoded into mev-commit-geth.

The fee vault contract is initiated with an immutable treasury address. The contract owner can invoke transfers of the accumulated fees from the sidechain contract, to the treasury address on L1, including bridging.

Since the treasury address is immutable, the contract owner only has the power to initiate transfers. The separation between the contract owner and treasury accounts is useful because key management can be separate. I.e. a secure multisig governing the protocol treasury doesn't need to submit regular transfer transactions.

## Deploying Custom Contracts

You can deploy your own custom contracts on our mev-commit chain. To do so, simply set your **Chain ID to 17864** and RPC Url to `http://69.67.151.95:8545`. Now you can deploy any EVM compatible contracts to the mev-commit chain. Feel free to use Foundry, Hardhat, or any other smart-contract development and deployment framework.
