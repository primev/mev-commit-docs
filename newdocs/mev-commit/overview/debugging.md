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

# Debugging

### **Debugging Tools in mev-commit Node**

As the mev-commit node is under active development, it includes debugging tools for in-depth analysis. These tools, primarily intended for developers and not for everyday users, offer insights into the node's functionality. They are accessible via the HTTP API endpoint.

### **Understanding Network Topology**

You can view the network's topology as recognized by the mev-commit node at the `/topology` endpoint. This provides information such as the number of connected nodes and their roles, helping to ensure your node is well-connected within the network.

Example of Topology Data:

```
{  "self": {    "Addresses": [      "/ip4/127.0.0.1/tcp/13522",      "/ip4/172.29.18.2/tcp/13522"    ],    "Ethereum Address": "0xB8c29FfD307067471fd7997873DfC496Fd02956f",    "Peer Type": "bootnode",    "Underlay": "16Uiu2HAmLYUvthfDCewNMdfPhrVefBbsfaPL22fWWfC2zuoh5SpV"  },  "connected_peers": {    "bidders": [      "0xb9286cb4782e43a202bfd426abb72c8cb34f886c"    ]  },  "blocked_peers": [    {      "Peer": "0x48ddc642514370bdafad81c91e23759b0302c915",      "Reason": "insufficient stake",      "Duration": "1m58.244219375s"    }  ]}
```

### **Prometheus Metrics for Monitoring**

The node emits various Prometheus metrics, which can be utilized to create dashboards displaying the node's statistics. These include default metrics from the `libp2p` networking library and are available at the `/metrics` endpoint.

### **Pprof Endpoints**

For advanced users, pprof endpoints are available at `/debug/pprof` for performance profiling, such as monitoring memory and CPU usage. However, understanding and using these endpoints requires prior knowledge of pprof tools and is not covered in this documentation.
