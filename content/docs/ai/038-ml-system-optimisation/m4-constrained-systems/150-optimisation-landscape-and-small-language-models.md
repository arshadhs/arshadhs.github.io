---
title: "Optimisation Landscape and Small Language Models"
draft: false
tags: ["MLSysOps", "Optimisation", "Small Language Models", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1500
menu:
  main:
    identifier: mlsysops_ch150
    parent: mlsysops_m4
---

# Optimisation Landscape and Small Language Models

ML systems can be improved at the model, algorithm, software, or hardware level. This introductory overview covers:

- Continued federated learning
- Generic, problem-specific, and platform-specific optimisation
- Frameworks, libraries, hardware, and compilers
- Small language models and microcontroller deployment

## 1. Federated Learning in Practice

- **Continued federated learning:** studies how training behaves with unequal participant data, intermittent devices, and limited communication. Useful evaluation considers client-level quality as well as overall model quality.

## 2. Choosing the Level of Optimisation ☆

- **Generic approaches:** broadly applicable techniques such as profiling, batching, caching, and reducing unnecessary data movement.
- **Specific approaches:** changes chosen for a particular workload, model, dataset, or deployment constraint, such as exploiting sparse embeddings.
- **Algorithm-specific optimisation:** exploits mathematical structure, such as replacing a redundant matrix with lower-rank factors.
- **Target-platform-specific optimisation:** adapts execution to a device's memory hierarchy, supported precision, instruction set, and accelerators.

## 3. The Software and Hardware Stack

- **ML frameworks:** organise models, automatic differentiation, training, and execution; examples include PyTorch and TensorFlow.
- **Libraries:** supply optimised building blocks, such as convolution, matrix multiplication, collective communication, or microcontroller inference kernels.
- **Hardware:** CPUs, GPUs, NPUs, and microcontrollers offer different balances of throughput, memory, latency, and energy use.
- **Compilers:** transform computation into executable operations, potentially fusing kernels, selecting layouts, or targeting specialised hardware instructions.

Optimise the measured bottleneck: a faster arithmetic kernel may provide little benefit when data transfer or input loading dominates.

## 4. Small Language Models and Tiny Runtimes

- **Small language models, SLMs:** language models with fewer parameters or lower deployment demands than larger alternatives. There is no universal size boundary; suitability depends on task and hardware.
- **TensorFlow Lite Micro / LiteRT for Microcontrollers:** a small inference runtime for supported models on microcontrollers with limited memory and resources.

These are separate ideas: a model described as an SLM may still need far more memory than a microcontroller provides. Runtime support and the complete memory budget must be checked.

## References and Further Reading

- [Google AI Edge: LiteRT for Microcontrollers](https://developers.google.com/edge/litert/microcontrollers/overview)
- [Google AI Edge: model optimisation](https://developers.google.com/edge/litert/conversion/tensorflow/quantization/model_optimization)
- [Arm: CMSIS-NN kernels](https://github.com/ARM-software/CMSIS-NN)
- [NVIDIA: CUDA Programming Guide](https://docs.nvidia.com/cuda/cuda-programming-guide/)
- [Google Cloud: federated learning](https://cloud.google.com/discover/what-is-federated-learning)

---
{{< home-link "Home" >}} | {{< section-index >}}
