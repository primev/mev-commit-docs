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

# Bundles

The bid structure allows for the submission of bids associated with continuous atomic transactions, also known as bundles.

This involves us updating the payload as follows:

Here's a simple bid for a single transaction hash
```
{
    "txHashes": ["0549fc7c57fffbdbfb2cf9d5e0de165fc68dadb5c27c42fdad0bdf506f4eacae"],
    "amount": "100000",
    "blockNumber": 123459
}
```

Here's a bid for a bundle:
```
{
    "txHashes": ["0549fc7c57fffbdbfb2cf9d5e0de165fc68dadb5c27c42fdad0bdf506f4eacae", 22145ba31366d29a893ae3ffbc95c36c06e8819a289ac588594c9512d0a99810, 7e1506f266bc86c81ae46018053a274a3bd96a9eff17392930707bf8fa5ff6be],
    "amount": "100000",
    "blockNumber": 123459
}
```

As you can see the bid now contains the following list: `["0549fc7c57fffbdbfb2cf9d5e0de165fc68dadb5c27c42fdad0bdf506f4eacae", 22145ba31366d29a893ae3ffbc95c36c06e8819a289ac588594c9512d0a99810, 7e1506f266bc86c81ae46018053a274a3bd96a9eff17392930707bf8fa5ff6be]`

This means that the provider that commits must ensure that the txns 0x0549...,0x2214...,0x7e1506... all occur in atomic sequence. If the trasnactions are out of order, or if there are transactions in between the three, the provider will be slashed.