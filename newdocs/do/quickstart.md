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

# Quickstart

This section provides a fast and straightforward method to start using mev-commit. It's designed for users who prefer a quick setup without delving into the technical details. Follow these simple steps to get started:

**Pre-requisites:**

* Ensure you have `curl` installed on your system. `curl` is a command-line tool used to transfer data from or to a server.
* A `terminal` to execute the command. This can be any command-line interface like Command Prompt on Windows, Terminal on macOS, or a Linux shell.

Simply run the following command in your terminal:

```bash
curl -o launchmevcommit https://raw.githubusercontent.com/primevprotocol/scripts/main/launchmevcommit && chmod +x launchmevcommit && ./launchmevcommit --rpc-url http://69.67.151.95:8545 --node-type bidder
```

**Providers:** Change the last section of the code "--node-type bidder" to "--node-type provider" to set mev-commit up as a provider.  &#x20;

❗You will need to expose port 13523 on your local machine to allow the provider node to establish connections that are incoming to the node.

**This command performs several actions automatically:**

* Downloads the mev-commit startup script using `curl`.
* Makes the script executable with `chmod`.
* Executes the script to set up mev-commit with the required configurations.

Once the command execution is complete, your mev-commit bidder or provider node setup is done. You don't need to perform any additional manual setup steps.

If you want to perform a manual setup please skip to [Manual Start mev-commit](../developers/manual-start-mev-commit.md) section.
