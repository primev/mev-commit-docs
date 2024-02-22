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

# Send a bid

#### Sending Bids

Open a new terminal window. To send bids, we simply run the following command:

```bash
curl -X POST http://localhost:13523/v1/bidder/bid \
-d '{
    "txHashes": ["0549fc7c57fffbdbfb2cf9d5e0de165fc68dadb5c27c42fdad0bdf506f4eacae"],
    "amount": "<amount in wei>",
    "blockNumber": <integer blocknumber>
}'
```

To include bundles of transactions, add them in the atomic sequence in which they exist in the bundle, as follows:

```bash
curl -X POST http://localhost:13523/v1/bidder/bid \
-d '{
    "txHashes": ["0549fc7c57fffbdbfb2cf9d5e0de165fc68dadb5c27c42fdad0bdf506f4eacae", 22145ba31366d29a893ae3ffbc95c36c06e8819a289ac588594c9512d0a99810, 7e1506f266bc86c81ae46018053a274a3bd96a9eff17392930707bf8fa5ff6be],
    "amount": "<amount in wei>",
    "blockNumber": <integer blocknumber>
}'
```

💡You can change the values in the fields txHash, amount and blockNumber as desired.

*   **Optional - Use Postman to send bids**

    If you’ve got a Postman account and postman agent installed, you can use [the following link](https://primev.postman.co/workspace/Team-Workspace\~18870d84-94f0-4d1e-8163-db558f83d7e8/request/27192304-fab87a71-9722-46f8-825f-d9791ead6178?ctx=documentation\&tab=body) to send bids Via Postman.
