---
title: "GPU Architecture and Federated Learning"
draft: false
tags: ["MLSysOps", "GPU", "Federated Learning", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1100
menu:
  main:
    identifier: mlsysops_ch110
    parent: mlsysops_m3
---

# GPU Architecture and Federated Learning

This introductory overview connects parallel execution within a GPU with collaborative learning across separate data owners. Topics include:

- GPU architecture, threading, and matrix operations
- Federated learning, non-IID data, and privacy
- Secure aggregation and decentralised architectures

## 1. GPU Architecture and Threading

- **GPU architecture:** contains many processing units designed for parallel work, with registers, shared memory, caches, and device memory serving different access needs.
- **Kernel:** a function launched to execute across many GPU threads, each typically handling a different part of the data.
- **Threads, blocks, and grids:** CUDA groups threads into blocks and blocks into a grid. Threads within a block can cooperate using shared memory and synchronisation.
- **Warps and SIMT:** NVIDIA GPUs organise threads into warps of 32 using a single-instruction, multiple-thread execution model. Diverging branch paths can reduce execution efficiency.

## 2. Matrix Operations

- **Matrix addition:** adds matching elements independently, making it easy to divide across threads. Its performance is often limited by memory traffic.
- **Multithreaded matrix multiplication:** distributes output elements or tiles across threads; reusing input tiles in fast memory reduces repeated device-memory reads.

## 3. Federated Learning ☆

- **Federated learning:** trains a shared model through participants that retain their raw training data locally and exchange model updates instead.
- **Non-IID data:** participants hold different data distributions, such as different languages or patient populations. Their updates may disagree and converge unevenly.
- **Privacy goals:** reduce exposure of sensitive local data. Updates can still reveal information, so additional protections and a clear threat model may be necessary.
- **Secure aggregation:** a cryptographic protocol lets a coordinator recover a combined update without seeing each participant's individual update, under the protocol's assumptions.

{{< mermaid >}}
flowchart TD
    A["Client A: local training"] -->|"Protected update"| S["Secure aggregation"]
    B["Client B: local training"] -->|"Protected update"| S
    S --> G["Updated shared model"]
    G -->|"Next round"| A
    G -->|"Next round"| B
    style A fill:#E1F5FE
    style B fill:#E1F5FE
    style S fill:#FFF9C4
    style G fill:#C8E6C9
{{< /mermaid >}}

Raw training examples stay at the clients in this illustration. Secure aggregation protects individual updates; it does not by itself prevent every form of leakage from the final model.

## 4. Decentralised Architectures

- **Decentralised federated learning:** replaces a single central coordinator with peer collaboration while data remain with their owners.
- **Gossip:** participants exchange and mix model information with selected neighbours, gradually spreading information through the network.
- **Blockchain-based architectures:** use a distributed ledger and consensus to coordinate or verify learning contributions. They introduce communication and consensus costs and do not automatically guarantee privacy.

## References and Further Reading

- [Google Cloud: what is federated learning?](https://cloud.google.com/discover/what-is-federated-learning)
- [NVIDIA: CUDA Programming Guide](https://docs.nvidia.com/cuda/cuda-programming-guide/)
- [Google Research: practical secure aggregation](https://research.google/pubs/practical-secure-aggregation-for-privacy-preserving-machine-learning/)
- [McMahan et al.: federated averaging](https://arxiv.org/abs/1602.05629)
- [Kim et al.: Blockchained On-Device Federated Learning](https://arxiv.org/abs/1808.03949)

---
{{< home-link "Home" >}} | {{< section-index >}}
