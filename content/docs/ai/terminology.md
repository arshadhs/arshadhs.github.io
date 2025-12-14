---

---
title: "AI Stages: ANI, AGI, ASI"
featured_image: '/images/zebrato.jpeg'
date: 2024-07-04T10:55:52+01:00
draft: false
tags: ["AI", "ANI", "AGI", "ASI"]
categories: ["AI", "ML"]
weight: 10
menu: main
---

# AI Terminology

{{<mermaid>}}
stateDiagram-v2
    State1: AI
	State1 --> ANI
	ANI: ANI - Artificial Narrow Intelligence
	note right of ANI
		GAI - Generative AI
	end note
	State1 --> 	AGI
	AGI: AGI - Artificial General Intelligence
	State1 --> ASI
	ASI: ASI - Artificial Super Intelligence


{{</mermaid>}}

## **Artificial Narrow Intelligence** 
ANI is also called **Weak AI** is trained on well labeled data sets to perform specfic task and operate within predefined environment. Doesn't go beyond it's designed purpose. Such as image and speech recognition. e.g. Siri, Alexa.

### **Generative AI** 
GAI is built using deep-learning creates new content such as text, images, music or videos by learning from vast datasets. e.g. ChatGPT.

## **Artificial General Intelligence**
AGI is also called strong AI is designed to perform a wide range of intelligent tasks, think abstract and adapt to new situations. e.g. Self Driving Cars to some extent.

## **Artificial Super Intelligence**
ASI represents hypothetical future where machine outsmarts human intelligence.

---

## AI Stages: ANI → AGI → ASI

These terms describe increasing levels of AI capability:

- **ANI (Artificial Narrow Intelligence):** specialised systems that perform specific tasks well (e.g., spam filters, speech recognition).
- **AGI (Artificial General Intelligence):** a system that can learn and reason across many tasks like a human-level general thinker.
- **ASI (Artificial Superintelligence):** intelligence that surpasses human capability across most domains.

---

## Mermaid Diagram

```mermaid
flowchart LR
  A["ANI<br/>Artificial Narrow Intelligence<br/><small>Task-specific intelligence</small>"] --> B["AGI<br/>Artificial General Intelligence<br/><small>Human-level general intelligence</small>"] --> C["ASI<br/>Artificial Superintelligence<br/><small>Beyond human intelligence</small>"]

  A --- A1["Examples:<br/>• Face recognition<br/>• Recommendations<br/>• Translation<br/>• Fraud detection"]
  B --- B1["Expected traits:<br/>• Transfers learning across domains<br/>• Reasoning + planning<br/>• Broad adaptability"]
  C --- C1["Implications:<br/>• Rapid problem-solving<br/>• High impact on society<br/>• Requires strong safety controls"]

---
{{< home-link "Home" >}} | {{< section-index >}}  
