---
title: "Energy-Constrained ML"
draft: false
tags: ["MLSysOps", "Energy Efficiency", "TinyML", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1600
menu:
  main:
    identifier: mlsysops_ch160
    parent: mlsysops_m4
---

# Energy-Constrained ML

Energy-constrained ML aims to produce useful predictions within a device's power, battery, memory, and timing limits. This introductory overview covers:

- Adapting algorithms to energy and resource constraints
- Accuracy, model size, throughput, and response time
- Energy consumption and performance trade-offs

## 1. Adapting the Workload

- **Energy constraints:** limit how much electrical energy a system can spend over an operation or operating period, such as a battery-powered sensor's daily budget.
- **Resource-aware algorithms:** choose model complexity, precision, sampling frequency, and execution strategy to match available compute and memory.
- **Data reuse and locality:** keep useful values near the processor and avoid repeated transfers. Memory movement can consume substantial energy as well as time.
- **Duty cycling:** performs sensing or inference when needed and lets hardware sleep between operations, reducing active time where the application allows it.

## 2. Performance Measures ☆

- **Prediction accuracy:** how often predictions are correct on a defined dataset; task-specific measures such as recall may also matter.
- **Model size:** the bytes required to store the model. Runtime memory also includes activations, buffers, and other execution state.
- **Throughput:** predictions completed per unit time, such as images per second; batching may improve it while delaying individual requests.
- **Response time:** elapsed time from an input or request to its result, including relevant waiting, transfer, and computation.
- **Power:** the rate of energy use, measured in watts. A low-power device can still consume substantial energy if it runs for a long time.
- **Energy per prediction:** electrical energy used to produce a result, measured in joules; for constant average power, energy equals power multiplied by runtime.

## 3. Understanding Trade-offs

- **Accuracy versus cost:** a smaller or lower-precision model may use fewer resources but change prediction quality; check the application requirements.
- **Speed versus energy:** finishing sooner can save energy, but higher power during execution can offset that saving.
- **Local versus remote computation:** local inference avoids some communication and network delay; cloud execution offers more compute but requires data transfer and connectivity.
- **End-to-end measurement:** include the relevant sensor, memory, communication, and idle costs, rather than measuring only the model's arithmetic.

**Small example:** 2 W for 0.1 s uses **0.2 J**; 0.5 W for 0.8 s uses **0.4 J**. The lower-power option uses more energy per prediction in this comparison.

## References and Further Reading

- [MIT: Eyeriss energy-efficient neural-network accelerator](https://eyeriss.mit.edu/)
- [MIT: reducing data-movement energy in CNNs](https://dspace.mit.edu/entities/publication/f6342515-2b7d-411c-a8ad-a9edb23dd394)
- [Google AI Edge: LiteRT for Microcontrollers](https://developers.google.com/edge/litert/microcontrollers/overview)
- [Google AI Edge: model optimisation](https://developers.google.com/edge/litert/conversion/tensorflow/quantization/model_optimization)

---
{{< home-link "Home" >}} | {{< section-index >}}
