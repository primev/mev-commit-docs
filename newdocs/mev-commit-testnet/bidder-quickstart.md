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

# Bidder Quickstart

This section is tailored for users who want to participate in the mev-commit network as bidders. Bidders are crucial participants who submit bids for transaction execution. There are two approaches to becoming a bidder: the 1-Minute Quickstart for those who prefer a rapid setup, and the Manual Start for those who seek more control and understanding of the setup process.

#### 1-Minute Quickstart

This section provides a fast and straightforward method to start using mev-commit as a bidder. It's designed for users who prefer a quick setup without delving into the technical details. Follow these simple steps to get started:

**Pre-requisites:**

* Ensure you have `curl` installed on your system. `curl` is a command-line tool used to transfer data from or to a server.
* A `terminal` to execute the command. This can be any command-line interface like Command Prompt on Windows, Terminal on macOS, or a Linux shell.

Simply run the following command in your terminal:

```bash
curl -o launchmevcommit https://raw.githubusercontent.com/primevprotocol/scripts/main/launchmevcommit && chmod +x launchmevcommit && ./launchmevcommit --rpc-url http://69.67.151.95:8545 --node-type bidder
```

**This command performs several actions automatically:**

* Downloads the mev-commit startup script using `curl`.
* Makes the script executable with `chmod`.
* Executes the script to set up mev-commit with the required configurations.

Once the command execution is complete, your mev-commit bidder node setup is done. You don't need to perform any additional manual setup steps.

If you want to perform a manual setup please skip to [Bidder Manual Start ](bidder-quickstart.md#bidder-manual-start)section.

#### Interacting with the Bidder Node

First, you can set the local environment variables to set your KEY and ADDRESS. Change directories to the one with your key (this should be the same folder as mev-commit). If you used the 1-minute quickstart command, your folder would be $HOME/.mev-commit

Setting these will make running the commands to interact with your account easier.

```bash
export KEY=$(cat key)
export ADDRESS=$(cast wallet address --private-key 0x$(cat key))
```

#### Sending Bids

Open a new terminal window. To send bids, we simply run the following command:

```bash
curl -X POST http://localhost:13523/v1/bidder/bid \
-d '{
  "txHash": "91a89B633194c0D86C539A1A5B14DCCacfD47094",
  "amount": <amount in wei>,
  "blockNumber": <blocknumber>
}'
```

💡You can change the values in the fields txHash, amount and blockNumber as desired.

*   **Optional - Use Postman to send bids**

    If you’ve got a Postman account and postman agent installed, you can use [the following link](https://primev.postman.co/workspace/Team-Workspace\~18870d84-94f0-4d1e-8163-db558f83d7e8/request/27192304-fab87a71-9722-46f8-825f-d9791ead6178?ctx=documentation\&tab=body) to send bids Via Postman.

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

## Bidder Manual Start

This section is for users who prefer a more hands-on approach to setting up mev-commit. It allows for customization and a better understanding of the setup process.

**Pre-requisites:**

* `curl`: A command-line tool used for downloading files from the internet.
* `terminal`: A command-line interface for executing commands.
* `cast`: A command-line tool for Ethereum blockchain interaction. Install it using the following command:

```bash
curl -L https://foundry.paradigm.xyz | bash
```

After installing `cast`, run `foundryup` to ensure it's updated.

#### Running the Bidder Node

*   **Step 1. Check your CPU architecture:**

    ```bash
    uname # This will give you the OS type
    uname -m # This will give you the chip architecture
    ```
*   **Step 2. Pull The Required Binary of mev-commit based on your CPU architecture.**

    Download the binaries from the following:

    https://github.com/primevprotocol/mev-commit/releases/tag/v0.1.0

    Unzip the binary and store it in a root directory of your choice.
*   **Step 3. Place the binary, named mev-commit, into a folder of your choice.**

    Note this will be considered the root folder of your mev-commit.
*   **Step 4. Run the initialization**

    cd into the folder that contains the mev-commit binary and run the following command:

    ```bash
    ./mev-commit init --dir . --rpc-endpoint http://69.67.151.95:8545  --peer-type bidder
    ```

    This will create both a private key file and a config.yaml file and store it in your current directory.

    While in the directory, run the following command to export relevant data about your address and private key:

    ```bash
    export KEY=$(cat key)
    export ADDRESS=$(cast wallet address --private-key 0x$(cat key))
    ```
*   **Step 5. Fund the account**

    cd to the folder where you’ve stored the key and run the following command to transfer 100 ether into your wallet from a faucet account:

    ❗If you’ve named the private key file something other than key, change the key to the correct name in \`--private-key 0x$(cat \*\*\*key\*\*\*))\`

    ```bash
    cast send --rpc-url [http://69.67.151.95:8545](http://69.67.151.95:8545/) --private-key 0x7c9bf0f015874594d321c1c01ada3166c3509bbd91f76f9e4d7380c2df269c55 $(cast wallet address --private-key 0x$(cat key)) --value 100ether
    ```
*   **Step 6. Run the node**

    1. cd into the folder with your mev-commit binary
    2. Run the Node:

    ```bash
    ./mev-commit start --config=~/config.yaml
    ```
*   **Step 7. Prepay for bids**

    The bidder needs to prepay a certain amount onto the Primev contracts to allow them to send bids and receive commitments. This is to ensure that providers can pull funds once the bids have been completed.

    To prepay, first **open a new terminal** and run the following command:

    ```bash
    curl -X POST http://localhost:13523/v1/bidder/prepay/1000000000000000000
    ```

    This will prepay `1000000000000000000 wei` into your account.
