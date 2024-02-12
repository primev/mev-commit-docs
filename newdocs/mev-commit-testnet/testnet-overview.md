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

# Testnet Overview

## Introduction to Testnet and its Purpose

The testnet allows bidders and providers to spin up nodes and send bids and commitments along. This will allow bidders to get pre-confirmations from providers on the Holesky network.

## **mev-commit chain connection details**

### **Services links**

| Service           | Link                                                       |
| ----------------- | ---------------------------------------------------------- |
| Bootnode endpoint | [http://34.215.163.180:8545/](http://34.215.163.180:8545/) |
| Bridge frontend   | [http://34.215.163.180/](http://34.215.163.180/)           |
| Block explorer    | [http://34.209.10.199/blocks](http://34.209.10.199/blocks) |

### **Contract addresses**

<table><thead><tr><th width="299">Contract</th><th>Address</th></tr></thead><tbody><tr><td>UserRegistry</td><td>0x390066a15e1048445F1B1b69Ba98AC4cb5e91c52</td></tr><tr><td>ProviderRegistry</td><td>0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC</td></tr><tr><td>PreConfCommitmentStore</td><td>0xBB632720f817792578060F176694D8f7230229d9</td></tr><tr><td>Oracle</td><td>0x51dcB14bcb0B4eE747BE445550A4Fb53ecd31617</td></tr><tr><td>Whitelist</td><td>0xc5bB85F941fb8dbbed6416A8aC84A06226E0f138</td></tr></tbody></table>



## Infrastructure

Primev will maintain a testnet consisting of the mev-commit chain, oracle service, and hyperlane bridge to Sepolia.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## Differences between the testnet and mainnet environments

Testnet will use Holesky and Holesky eth, whereas mainnet uses mainnet eth. Commitments are also written to the settlement in plaintext, whereas mainnnet will include private commitments.

## Limitations of the testnet

We do not yet support a decay mechanism that would allow for higher rewards to accrue to a provider when they submit a commitment earlier into a slot.

We also do not have bid and commitment privacy beyond the encrypted channels that exist in the p2p communication across the network. This means that when a commitment and bid are stored on the chain, they’re public.

## How users can provide feedback or report issues encountered on the testnet?

Users can open an issue on the GitHub repository that is associated with any issue they encounter. They can also tag @primev\_xyz on Twitter.

## Testnet System Requirements

Please refer to System Requirements section [here.](https://www.notion.so/Primev-Docs-Testnet-Ver-41c24e77a73e47e0966d54365efbbfc8?pvs=21)

## Setting Up Your Full Node for the mev-commit Testnet

To join the mev-commit chain testnet with your own full-node, use [mev-commit geth](https://github.com/primevprotocol/go-ethereum). We've modified go-ethereum to achieve shorter block periods than mainnet Ethereum, and to enable seamless native token bridging capabilities. Geth configuration will vary based on environment, but an example is provided below:

```
exec geth \\    --verbosity="$VERBOSITY" \\    --datadir="$GETH_DATA_DIR" \\    --port 30311 \\    --syncmode=full \\    --gcmode=full \\    --http \\    --http.corsdomain="*" \\    --http.vhosts="*" \\    --http.addr=0.0.0.0 \\    --http.port="$RPC_PORT" \\    --http.api=web3,debug,eth,txpool,net,engine \\    --bootnodes enode://34a2a388ad31ca37f127bb9ffe93758ee711c5c2277dff6aff2e359bcf2c9509ea55034196788dbd59ed70861f523c1c03d54f1eabb2b4a5c1c129d966fe1e65@172.13.0.100:30301 \\    --networkid=$CHAIN_ID \\    --unlock=$BLOCK_SIGNER_ADDRESS \\    --password="$GETH_DATA_DIR"/password \\    --mine \\    --miner.etherbase=$BLOCK_SIGNER_ADDRESS \\    --allow-insecure-unlock \\    --nousb \\    --netrestrict 172.13.0.0/24 \\    --metrics \\    --metrics.addr=0.0.0.0 \\    --metrics.port=6060 \\    --ws \\    --ws.addr=0.0.0.0 \\    --ws.port="$WS_PORT" \\    --ws.origins="*" \\    --ws.api=debug,eth,txpool,net,engine \\    --rpc.allow-unprotected-txs \\    --authrpc.addr="0.0.0.0" \\    --authrpc.port="8551" \\    --authrpc.vhosts="*" \\    --nat extip:$NODE_IP
```
