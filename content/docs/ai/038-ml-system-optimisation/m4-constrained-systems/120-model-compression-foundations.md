---
title: "Model Compression Foundations"
draft: false
tags: ["MLSysOps", "Model Compression", "Distillation", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1200
menu:
  main:
    identifier: mlsysops_ch120
    parent: mlsysops_m4
---

# Model Compression Foundations

Model compression makes neural networks easier to store, move, and execute. This introductory overview covers:

- GPU implementation of CNNs and compression techniques
- Storage, memory, and bandwidth savings
- Lossy and lossless compression, sharing, clustering, and coding
- Knowledge distillation and teacher–student learning

## 1. Computation and Compression ☆

- **GPU implementation of CNNs:** maps convolutions and related tensor operations onto parallel GPU kernels, usually through optimised libraries rather than one thread per entire image.
- **Quantisation:** represents weights or activations using fewer numerical levels or lower precision, reducing storage and potentially accelerating supported operations.
- **Network pruning:** removes or zeros selected weights or larger structures judged less useful, aiming to reduce the model's effective complexity.
- **Low-rank factorisation:** approximates a large weight matrix or convolution using smaller factors, exploiting redundancy to reduce parameters and arithmetic.

## 2. What Can Be Saved?

- **Storage savings:** reduce the bytes needed to save or download the model. A smaller file does not necessarily mean lower runtime memory.
- **Memory savings:** reduce weights, activations, or working buffers held during execution, allowing deployment on smaller devices.
- **Bandwidth savings:** move fewer bytes between memory levels, devices, or servers; this can help when data movement is a bottleneck.
- **Lossy compression:** changes represented values or model structure, potentially affecting predictions; quantisation and pruning are typical examples.
- **Lossless compression:** changes how information is encoded while permitting exact reconstruction of the encoded values, as with entropy coding.

## 3. Sharing, Clustering, and Encoding

- **Weight sharing:** several connections refer to the same stored value, reducing the number of unique parameters that need storage.
- **HashNet / HashedNets:** uses a hash function to map connections into a smaller set of shared weight buckets, avoiding a separate learned value for every connection.
- **Weight clustering:** groups similar weights and replaces each with a cluster representative; a codebook and cluster indices can encode the result.
- **Encoding:** stores values, indices, or codebook symbols in a compact representation. Index and metadata costs also contribute to the final size.
- **Huffman coding:** assigns shorter binary codes to more frequent symbols and longer codes to rarer symbols, enabling lossless compression.
- **Arithmetic coding:** represents a sequence through a progressively narrowed numerical interval based on symbol probabilities, rather than assigning each symbol a separate fixed codeword.

## 4. Knowledge Distillation

- **Teacher–student models:** a student learns to imitate a teacher's behaviour, often transferring useful predictions from a larger model into a smaller one.
- **Temperature:** adjusts the softness of the teacher's output probabilities; a higher softmax temperature exposes more information about less likely classes.
- **Soft labels:** probability distributions over classes, such as 70% cat and 25% dog, rather than only one correct class.
- **Cost functions:** distillation commonly combines ordinary labelled-data loss with a loss matching teacher and student distributions, such as KL divergence.

{{< mermaid >}}
flowchart TD
    T["Teacher predictions"] --> S["Soft targets"]
    S --> L["Combined training loss"]
    H["True labels"] --> L
    P["Student predictions"] --> L
    L -->|"Update student"| P
    style T fill:#EDE7F6
    style S fill:#E1F5FE
    style H fill:#FFF9C4
    style P fill:#C8E6C9
    style L fill:#E1F5FE
{{< /mermaid >}}

## References and Further Reading

- [Han et al.: Deep Compression](https://arxiv.org/abs/1510.00149)
- [Chen et al.: Compressing Neural Networks with the Hashing Trick](https://arxiv.org/abs/1504.04788)
- [Hinton et al.: Distilling the Knowledge in a Neural Network](https://arxiv.org/abs/1503.02531)
- [Jaderberg et al.: low-rank expansions for CNNs](https://arxiv.org/abs/1405.3866)
- [Google AI Edge: model optimisation](https://developers.google.com/edge/litert/conversion/tensorflow/quantization/model_optimization)

---
{{< home-link "Home" >}} | {{< section-index >}}
