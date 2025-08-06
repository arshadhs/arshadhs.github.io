---
title: 'Machine Learning'
featured_image: '/images/zebrato.jpeg'
date: 2024-08-06T23:29:52+01:00
draft: false
tags: ["AI", "ML"]
categories: ["AI", "ML"]
weight: 20
menu: main
---

# Machine Learning

---

{{<mermaid>}}
stateDiagram-v2
	classDef ahsState font-style:italic,font-weight:bold,fill:lightblue
    State1: Machine Learning
	State1 --> SL:::ahsState
	SL: Supervised Learning

	SL --> 	Classification
	Classification --> NB
	NB: Naive Bayes
	NB --> NN
	NN: Nearest Neighbour
	NN --> SVM
	SL --> 	Regression
	Regression --> LR
	LR: Linear Regression
	LR --> NNetwork
	NNetwork: Neural Network
	NNetwork --> DT
	DT: Decision Tree
	State1 --> USL:::ahsState
	USL: Unsupervised Learning
	USL --> Clustering
	Clustering --> KM
	KM: K-Means
	KM --> GM
	GM : Gaussian Matrix
	note right of GM
		Neural Networks
	end note
	GM --> HM
	HM : Hidden Markov
	State1 --> Reinforcement:::ahsState
	Reinforcement --> DM
	DM: Decision Making

{{</mermaid>}}

**Supervised Learning** uses labeled data and output training data.

**Unsupervised Learning** learns relationship and patterns from unlabelled raw data.

**Reinforcement** uses rewards and punishment model to train. The agent learns a policy/strategy that maximises its rewards over time.

---
{{< home-link "Home" >}} | {{< section-index >}}