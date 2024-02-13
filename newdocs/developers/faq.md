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

# FAQ

## What is a Prepay amount?

The prepay amount, or allowance, is a deposit bidders make to enable bidding. For instance, to place 10 bids of 100 gwei each, a bidder would deposit 1000 gwei as a prepay. This helps the provider monitor preconfirmed bids and ensures bidders don't exceed their specified allowance.

## I’m getting the following error: “failed to read msg: stream reset”&#x20;

If you’re a provider node, this is likely due to a lack of staking and registration on the provider contract. Follow the instructions under _Step 7. Stake Funds and Register,_ to resolve this issue.

## Why does cast need to be used?

Cast is used in various situations to allow users to interact with the mev-commit chain. However, you can feel free to use any EVM-compatible tooling and set the RPC URL accordingly.

## What is Foundry? Foundry is a suite of tools that allows for both smart contract development and interactions. It’s used extensively for the deployment, testing, and interactions with mev-commit contracts.

Do I need to install Cast? No, however, it can make doing things like transferring funds for interacting with smart contracts easier.

## Why do I get the same commitment back when I resend a bid?

This is because the commitments work based on hashes and ECDSA signature schemes. These schemes are deterministic, such that as long as the bid payload and bidder private key remains the same, and the provider private key remains the same, the output commitment will be the same, even if the bid is committed to more than once.
