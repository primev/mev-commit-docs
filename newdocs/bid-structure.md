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

# Bid Structure

The bid payload sent to the internal mev-commit API is as follows:

```bash
# Example Bid Structure:
# txn_hash: arbitrary string value representing txn hash
# bid_amt: amount to bid in wei
# block_number: target block to have bid included
# bid_digest: hash of the above bid keccak{txn_hash, bid_amt, block_number}
# bid_signature: Signed bid_digest = sign(bid_digest, signing_key)
{
   "txn_hash":"0x8d562df4f0363de75c62441d69c46aa6c9144a6e9e4697509b3b24bf8a694322",
   "bid_amt":277610,
   "block_number":2703,
   "bid_digest":"uU2p20f5KmehWqpuY1u+CbhcS8jNwdQAJQe2dh0Vnrk=",
   "bid_signature":"nY6jYsGPxj6LVlSVQJbZcxvmRrw8Ym5rqOL1x0W/xPlJGBaF/ZzzjkxiioY/MDiRGvlflSWeoT0fh3aIJiJxAhw="
}
```

The final bid structure includes a hash and signature that is auto-constructed by your mev-commit bidder node.

Note that the prepay amount a bidder has set needs to be 10 times higher than the bid\_amt in the bid. We will be introducing a prepay mechanism in the next testnet milestone.
