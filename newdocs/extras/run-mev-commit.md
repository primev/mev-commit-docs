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

# Run mev-commit

mev-commit software operates as a command-line interface (CLI) tool, designed for users to run in their own environments. The CLI mainly includes two commands. To view available commands, use the `-h` or `--help` option with the main command.

Example:

```sql
NAME:
   mev-commit - Start mev-commit node

USAGE:
   mev-commit [global options] command [command options] 

VERSION:
   v0.1.2

COMMANDS:
   help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --config value                           path to config file [$MEV_COMMIT_CONFIG]
   --peer-type value                        peer type to use, options are 'bidder', 'provider' or 'bootnode' (default: "bidder") [$MEV_COMMIT_PEER_TYPE]
   --priv-key-file value                    path to private key file (default: "~/.mev-commit/key") [$MEV_COMMIT_PRIVKEY_FILE]
   --keystore-password value                use to access keystore [$MEV_COMMIT_KEYSTORE_PASSWORD]
   --keystore-path value                    path to keystore location (default: "~/.mev-commit/keystore") [$MEV_COMMIT_KEYSTORE_PATH]
   --p2p-port value                         port to listen for p2p connections (default: 13522) [$MEV_COMMIT_P2P_PORT]
   --p2p-addr value                         address to bind for p2p connections (default: "0.0.0.0") [$MEV_COMMIT_P2P_ADDR]
   --http-port value                        port to listen for http connections (default: 13523) [$MEV_COMMIT_HTTP_PORT]
   --http-addr value                        address to bind for http connections [$MEV_COMMIT_HTTP_ADDR]
   --rpc-port value                         port to listen for rpc connections (default: 13524) [$MEV_COMMIT_RPC_PORT]
   --rpc-addr value                         address to bind for RPC connections [$MEV_COMMIT_RPC_ADDR]
   --bootnodes value [ --bootnodes value ]  list of bootnodes to connect to [$MEV_COMMIT_BOOTNODES]
   --secret value                           secret to use for signing (default: "secret") [$MEV_COMMIT_SECRET]
   --log-fmt value                          log format to use, options are 'text' or 'json' (default: "text") [$MEV_COMMIT_LOG_FMT]
   --log-level value                        log level to use, options are 'debug', 'info', 'warn', 'error' (default: "info") [$MEV_COMMIT_LOG_LEVEL]
   --bidder-registry-contract value         address of the bidder registry contract (default: "0xa86a41b57Fb73f9118F84847574517258d29eAD0") [$MEV_COMMIT_BIDDER_REGISTRY_ADDR]
   --provider-registry-contract value       address of the provider registry contract (default: "0x5960774AD41D03DAB4916a30bD2190f8b3b3b4b2") [$MEV_COMMIT_PROVIDER_REGISTRY_ADDR]
   --preconf-contract value                 address of the preconfirmation commitment store contract (default: "0x7D1a4707e573D260581f3AB3f90f697Ab03fC6Dd") [$MEV_COMMIT_PRECONF_ADDR]
   --settlement-rpc-endpoint value          rpc endpoint of the settlement layer (default: "http://localhost:8545") [$MEV_COMMIT_SETTLEMENT_RPC_ENDPOINT]
   --nat-addr value                         external address of the node [$MEV_COMMIT_NAT_ADDR]
   --nat-port value                         externally mapped port for the node (default: 13522) [$MEV_COMMIT_NAT_PORT]
   --server-tls-certificate value           Path to the server TLS certificate [$MEV_COMMIT_SERVER_TLS_CERTIFICATE]
   --server-tls-private-key value           Path to the server TLS private key [$MEV_COMMIT_SERVER_TLS_PRIVATE_KEY]
   --help, -h                               show help
   --version, -v                            print the version
```

The options above have default values. The only arguments required for all nodes are:

* `--bootnodes` This should point to the bootnode for the Primev testnet network.
* `--settlement-rpc-endpoint` This should point to the Primev Settlement Chain RPC endpoint. At present this is managed by the Primev team.

The default contract addresses point to the Primev testnet at the moment. All the options can be provided using a YAML config file or environment variables as described in the above help output.

### If you're not using Docker:

1. Run Make in the root directory
2. This will compile the mev-commit binary under the bin folder

```bash
mev-commit % tree -L 2
.
├── Dockerfile
├──...
├── bin
│   ├── config.yaml
│   ├── key
**│   └── mev-commit**
```

1. Run the mev-commit directly:

```bash

RPC_URL="<http://69.67.151.95:8545>";bin/mev-commit --peer-type <peerType> --bootnodes <bootnode> --settlement-rpc-endpoint $RPC_URL
```

### If you're using Docker:

1. **Build Docker Image**:
   * Open your terminal and navigate to where your Dockerfile is located.
   *   Run:

       ```sql
       sqlCopy code
       docker build -t mev-commit:latest .

       ```

