# Registering as a Provider

In order to register as a provider in the mev-commit network, user has to stake some ETH on the provider registry. This gives us a way to penalize nodes that are misbehaving. For, e.g. preconfirming bids and not adding the transactions to the block that was built. If the provider node is not staked, other nodes will not be connected to it. Once it is staked, it will be allowed into the network and start seeing bidders connecting to it to send bids.

Users can use the provider API to check and add stake.

*   Check the stake

    ```bash
    > curl localhost:13523/v1/provider/get_stake | jq
    {
      "amount": "0"
    }
    ```

    In order to stake, we need to first add funds to the Ethereum wallet created for the node.

    *   Get the Ethereum wallet address of the node

        ```bash
        > curl localhost:13523/topology | jq
        {
          "self": {
            "Addresses": [
              "/ip4/127.0.0.1/tcp/13522",
              "/ip4/172.29.0.4/tcp/13522"
            ],
            **"Ethereum Address": "0xB9286CB4782E43A202BfD426AbB72c8cb34f886c"**,
            "Peer Type": "provider",
            "Underlay": "16Uiu2HAmDWZb4DxZQkS9yseXNukBFe6MhZdimSKuZcHFeJrF3jC9"
          },
          "connected_peers": null,
          "blocked_peers": null
        }
        ```
    *   Add funds to the account. Currently for the testnet, you can use the Primev account to get some testnet ETH. This can be done using the cast command:

        ```bash
        > cast send --rpc-url <https://rpc.mev-commit-chain.xyz> \\
        	--private-key 0x7c9bf0f015874594d321c1c01ada3166c3509bbd91f76f9e4d7380c2df269c55 \\
        	$ADDRESS \\
        	--value 100ether
        ```

        This command will transfer 100 ETH to your `$ADDRESS` which should be enough to get started.
*   Check the minimum amount that can be staked. Bidders will only connect to providers which have stake higher than this minimum. The amount is in `wei`

    ```bash
    > curl localhost:13523/v1/provider/get_min_stake | jq
    {
      "amount": "1000000000000000000"
    }
    ```
*   Stake

    ```bash
    > curl -X POST localhost/v1/provider/register_stake/10000000000000000000 | jq
    {
      "amount": "10000000000000000000"
    }
    ```

Once you have staked, wait for a few minutes, and you should see peers getting connected in the logs. You can check the `/topology` endpoint again to verify that you have some connected peers.
