---
title: "CNN Pipeline"
date: 2026-04-22
draft: false
weight: 657
tags: ["deep-learning", "cnn", "keras"]
categories: ["ai", "deep-learning"]
---

# CNN Pipeline: Preprocessing & Models

- Understand CNN concepts deeply
- Build CNN models step-by-step
- Apply CNNs in assignments using Keras

{{% hint info %}}
Think of CNN as a pipeline:
Image → Features → Patterns → Prediction
{{% /hint %}}

---

# 1. Image Representation

{{% colour "green" %}}
{{< katex display=true >}}
X \in \mathbb{R}^{H \times W \times C}
{{< /katex >}}
{{% /colour %}}

- H = Height  
- W = Width  
- C = Channels  

---

# 2. Convolution Operation

{{% colour "green" %}}
{{< katex display=true >}}
Z(i,j) = \sum_{m,n} X(i+m, j+n) \cdot K(m,n)
{{< /katex >}}
{{% /colour %}}

- Sliding filter extracts features  
- Produces feature maps  

---

# 3. Stride & Padding

{{% colour "green" %}}
{{< katex display=true >}}
Output = \frac{N - F + 2P}{S} + 1
{{< /katex >}}
{{% /colour %}}

---

# 4. Activation (ReLU)

{{% colour "green" %}}
{{< katex display=true >}}
ReLU(x) = max(0, x)
{{< /katex >}}
{{% /colour %}}

---

# 5. Pooling

- Max Pooling → strongest feature  
- Average Pooling → smooth  

---

# 6. Global Average Pooling

{{% colour "green" %}}
{{< katex display=true >}}
y_k = \frac{1}{HW} \sum_{i,j} x_{i,j,k}
{{< /katex >}}
{{% /colour %}}

---

# 7. Loss Function

{{% colour "green" %}}
{{< katex display=true >}}
L = - \sum y \log(\hat{y})
{{< /katex >}}
{{% /colour %}}

---

# 8. CNN Architecture

```mermaid
graph LR
A[Input Image] --> B[Conv]
B --> C[ReLU]
C --> D[Pooling]
D --> E[Conv Layers]
E --> F[Flatten / GAP]
F --> G[Dense]
G --> H[Output]
```

---

# 9. Training

- Forward pass  
- Loss computation  
- Backpropagation  
- Weight update  

---

# 10. Keras Implementation

## Model

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Dense, Flatten

model = Sequential()

model.add(Conv2D(32, (3,3), activation='relu', input_shape=(64,64,3)))
model.add(MaxPooling2D((2,2)))

model.add(Conv2D(64, (3,3), activation='relu'))
model.add(MaxPooling2D((2,2)))

model.add(Flatten())

model.add(Dense(128, activation='relu'))
model.add(Dense(1, activation='sigmoid'))
```

---

## Compile

```python
model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
```

---

## Train

```python
model.fit(X_train, y_train, epochs=10, batch_size=32)
```

---

## Predict

```python
pred = model.predict(X_test)
```

---

# 11. Tips

- Normalize images  
- Use small filters  
- Avoid too many dense layers  

---

# 12. Summary

{{% hint info %}}
CNN = Automatic feature extractor + classifier
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
