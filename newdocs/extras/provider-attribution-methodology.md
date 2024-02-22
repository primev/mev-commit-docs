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

# Provider Attribution Methodology

The current methodology for identifying the provider (also known as the builder) responsible for constructing the L1 block relies on the **`extra_data`** field within the block's execution payload. We correlate this information with the corresponding address registered in the mev-commit protocol. There are ongoing efforts to develop and implement more cryptographically secure methods of attribution.

## Oracle Roadmap

In the longer term, we plan to enable stronger provider attribution through the use of shortened signatures in the extra-data payload.

## Slashing Conditions

**Unsatisfied promise**: In the condition where a provider was meant to carry out a promise, and was able to carry out the promise (e.g was able to deliver payload into target slot), but refused to do so.
