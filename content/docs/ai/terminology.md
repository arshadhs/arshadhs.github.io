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
	ANI: ANI - Artificial Neural Intelligence
	note right of ANI
		GAI - Generative AI
	end note
	State1 --> 	AGI
	AGI: AGI - Artificial General Intelligence
	State1 --> ASI
	ASI: ASI - Artificial Super Intelligence


{{</mermaid>}}

**ANI** also called Weak AI is trained on well labeled data sets to perform specfic task and operate within predefined environment. Doesn't go beyond it's designed purpose. Such as image and speech recognition. e.g. Siri, Alexa.

**GAI** built using deep-learning creates new content such as text, images, music or videos by learning from vast datasets. e.g. ChatGPT.

**AGI** also called strong AI is designed to perform a wide range of intelligent tasks, think abstract and adapt to new situations. e.g. Self Driving Cars to some extent.

**ASI** represents hypothetical future where machine outsmarts human intelligence.


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
