# Bid commands

#### Interacting with the Bidder Node

First, you can set the local environment variables to set your KEY and ADDRESS. Change directories to the one with your key (this should be the same folder as mev-commit). If you used the 1-minute quickstart command, your folder would be $HOME/.mev-commit

Setting these will make running the commands to interact with your account easier.

```bash
export KEY=$(cat key)
export ADDRESS=$(cast wallet address --private-key 0x$(cat key))
```

#### Getting Allowance

To get the current prepaid balance in the contract:

```bash
curl -s http://localhost:13523/v1/bidder/get_allowance
```

💡Allowance represents the funds in the bidder's account that can be used to submit bids on the Primev p2p-network and settled on-chain.

#### Withdraw Prepaid Funds

```bash
cast send 0x62197Abd7672925c7606Bdf9931e42baCa6619AD "withdrawPrepayedAmount(address)" $ADDRESS --rpc-url http://69.67.151.95:8545 --private-key $KEY
```

#### Checking the Balance of your Wallet

This command will allow you to check your current wallet balance on mev-commit chain:

```bash
cast b $ADDRESS --rpc-url http://69.67.151.95:8545
```

#### Check Total Value Locked in Contracts

```bash
python3 -c "print($(cast b 0x62197Abd7672925c7606Bdf9931e42baCa6619AD --rpc-url http://69.67.151.95:8545) + $(cast b 0xeA73E67c2E34C4E02A2f3c5D416F59B76e7617fC --rpc-url http://69.67.151.95:8545))"
```

#### Topology

```bash
curl http://localhost:13523/topology
```
