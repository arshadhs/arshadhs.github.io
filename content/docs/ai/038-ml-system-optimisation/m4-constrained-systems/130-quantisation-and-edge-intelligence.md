---
title: "Quantisation and Edge Intelligence"
draft: false
tags: ["MLSysOps", "Quantisation", "Edge Intelligence", "AI", "ML"]
categories: ["AI", "ML"]
weight: 1300
menu:
  main:
    identifier: mlsysops_ch130
    parent: mlsysops_m4
---

# Quantisation and Edge Intelligence ☆

Edge intelligence runs useful ML computation close to where data are produced. This introductory overview covers:

- Model compression, distillation, and edge use cases
- Uniform and non-uniform quantisation
- Post-training quantisation and quantisation-aware training
- Reduced-precision arithmetic and supporting hardware

## 1. Compression at the Edge

- **Continued model compression:** combines methods such as pruning, quantisation, and distillation to fit deployment limits; the methods can interact and need joint evaluation.
- **Knowledge distillation:** trains a smaller student using a teacher's outputs, aiming to retain useful behaviour while reducing deployment cost.
- **Edge intelligence case studies:** examples include keyword detection on a microcontroller, camera-based inspection, and wearable activity recognition; each has different memory, latency, and power constraints.

## 2. Quantisation Choices ☆

- **Uniform quantisation:** maps values onto evenly spaced levels using a scale and, where appropriate, a zero-point.
- **Non-uniform quantisation:** uses unevenly spaced levels, allocating more representational detail to selected value ranges, such as through a learned codebook.
- **Post-training quantisation, PTQ:** converts an already trained model to lower precision. Some forms use representative calibration data to estimate activation ranges.
- **Quantisation-aware training, QAT:** simulates quantisation effects during training or fine-tuning so the model can adapt before conversion to a quantised deployment format.

## 3. Reduced Precision and Training

- **16-bit arithmetic:** uses formats such as FP16 or bfloat16 instead of FP32 for selected operations; the formats have different range and precision characteristics.
- **8-bit arithmetic:** commonly represents weights or activations with integer values and scaling metadata; accumulation may use wider integers.
- **Binary arithmetic:** restricts selected values to two levels, often enabling specialised bitwise computation. Not every operation or intermediate value must be binary.
- **Ternary arithmetic:** restricts selected values to three levels, commonly negative, zero, and positive scaled values.
- **Forward-pass effects:** rounding or clipping can change activations and predictions; suitable kernels may reduce memory traffic and execution time.
- **Backward-pass effects:** quantisation complicates differentiation, so QAT often uses approximate gradient rules while keeping training quantities in floating point.

## 4. Hardware Support

- **Vector units:** process multiple values per instruction; performance depends on supported data types and whether software uses the available instructions.
- **bfloat16:** a 16-bit floating-point format with FP32-like exponent range but fewer precision bits, useful for reducing data size in supported ML operations.
- **MCU DSP blocks and instructions:** accelerate operations such as multiply–accumulate on microcontrollers; libraries such as CMSIS-NN provide suitable neural-network kernels.

Lower precision provides speed benefits only when the deployment runtime and hardware support the chosen representation effectively.

## References and Further Reading

- [TensorFlow: quantisation-aware training](https://www.tensorflow.org/model_optimization/guide/quantization/training)
- [Google AI Edge: model optimisation](https://developers.google.com/edge/litert/conversion/tensorflow/quantization/model_optimization)
- [Google AI Edge: LiteRT for Microcontrollers](https://developers.google.com/edge/litert/microcontrollers/overview)
- [Arm: CMSIS-NN](https://github.com/ARM-software/CMSIS-NN)
- [Google Cloud: bfloat16](https://cloud.google.com/tpu/docs/bfloat16)

---
{{< home-link "Home" >}} | {{< section-index >}}
