---
title: 'AI Terminology'
featured_image: '/images/zebrato.jpeg'
date: 2024-07-04T10:55:52+01:00
draft: false
tags: ["AI"]
categories: ["AI"]
weight: 10
menu: main
---

# AI Terminology

{{<mermaid>}}
stateDiagram-v2
    State1: AI
	State1 --> ANI
	ANI: ANI (Artificial Neural Intelligence)
	State1 --> 	GAI
	GAI: Generative AI
	State1 --> 	AGI

{{</mermaid>}}

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

---