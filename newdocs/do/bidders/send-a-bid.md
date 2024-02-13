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
  "txHash": "91a89B633194c0D86C539A1A5B14DCCacfD47094",
  "amount": <amount in wei>,
  "blockNumber": <blocknumber>
}'
```

💡You can change the values in the fields txHash, amount and blockNumber as desired.

*   **Optional - Use Postman to send bids**

    If you’ve got a Postman account and postman agent installed, you can use [the following link](https://primev.postman.co/workspace/Team-Workspace\~18870d84-94f0-4d1e-8163-db558f83d7e8/request/27192304-fab87a71-9722-46f8-825f-d9791ead6178?ctx=documentation\&tab=body) to send bids Via Postman.
