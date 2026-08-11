---
title: "Natural Language Processing"
draft: false
tags: ["Natural Language Processing", "NLP", "AI", "ML"]
categories: ["AI", "ML"]
weight: 050
menu: main
bookCollapseSection: true
---

# Natural Language Processing

Natural Language Processing (NLP) studies how computers can analyse, understand, represent, and generate human language.

It combines ideas from linguistics, computer science, machine learning, and deep learning to work with text and language-based information.

{{% hint info %}}
**Natural Language Processing = Linguistics + Computation + Machine Learning**
{{% /hint %}}

The learning path begins with language understanding and vector representations, progresses through language modelling, tagging, and parsing, and then moves towards transformers, knowledge graphs, Retrieval-Augmented Generation, and modern NLP applications.

---

## Big Picture

{{< mermaid >}}
flowchart TD
    A["Language Understanding"] --> B["Vector Semantics"]
    B --> C["Language Models"]
    C --> D["POS Tagging"]
    D --> E["Parsing"]
    E --> F["Attention and Transformers"]
    F --> G["Word Meaning"]
    G --> H["Knowledge Graphs"]
    H --> I["RAG"]
    I --> J["NLP Applications"]

    style A fill:#E1F5FE
    style B fill:#C8E6C9
    style C fill:#FFF9C4
    style D fill:#EDE7F6
    style E fill:#E1F5FE
    style F fill:#C8E6C9
    style G fill:#FFF9C4
    style H fill:#EDE7F6
    style I fill:#E1F5FE
    style J fill:#C8E6C9
{{< /mermaid >}}

---

## Modular Structure

### 1. Natural Language Understanding and Generation

- The study of language
- Applications of Natural Language Understanding
- Evaluation of language-understanding systems
- Levels of language analysis
- Organisation of NLP systems
- Natural language generation

### 2. Vector Semantics and Embeddings

- Lexical semantics
- Vector semantics
- Words and vectors
- TF-IDF
- Word2Vec
- Skip-gram and Continuous Bag of Words
- GloVe
- Visualisation of embeddings

### 3. N-gram Language Modelling

- N-grams
- Generalisation and zero probabilities
- Smoothing
- Stupid Backoff
- Evaluation of language models

### 4. Neural Networks and Neural Language Modelling

- Feed-forward neural networks
- Training neural networks for language modelling
- Neural language models
- Neural text generation

### 5. Large Language Models and Prompt Engineering

- Introduction to Large Language Models
- Foundations of prompt engineering
- Zero-shot and few-shot prompting
- Chain-of-thought prompting
- Generated-knowledge prompting

### 6. Part-of-Speech Tagging

- English word classes
- Penn Treebank tag set
- Part-of-Speech tagging
- Markov Chains
- Hidden Markov Models
- HMM-based Part-of-Speech tagging

### 7. Statistical, Machine-Learning, and Neural Models for Tagging

- Likelihood computation using the Forward Algorithm
- Decoding using the Viterbi Algorithm
- Maximum Entropy Markov Models
- Bidirectional models
- Neural-network models for Part-of-Speech tagging

### 8. Constituency Parsing

- Grammars and sentence structure
- Properties of a useful grammar
- Bottom-up Chart Parsing
- Probabilistic Context-Free Grammars
- Probabilistic CKY parsing
- Learning PCFG rule probabilities
- Limitations and improvements of PCFGs
- Lexicalised Context-Free Grammars

### 9. Dependency Parsing

- Dependency relations and formalisms
- Dependency treebanks
- Transition-based Dependency Parsing
- Graph-based Dependency Parsing
- Neural dependency parsers

### 10. Encoder-Decoder Models, Attention, and Contextual Embeddings

- Neural language generation
- Encoder-decoder networks
- Attention mechanisms
- Applications of encoder-decoder networks
- Self-attention
- Transformer networks
- BERT
- Contextual word representations

### 11. Word Sense Disambiguation

- Word senses
- Relations between senses
- WordNet and lexical relations
- Word Sense Disambiguation
- Alternative WSD algorithms and tasks
- Word Sense Induction

### 12. Semantic Web, Ontologies, and Knowledge Graphs

- Introduction to the Semantic Web
- Ontologies and Semantic Web languages
- Ontology engineering
- Ontology learning
- Knowledge-graph construction

### 13. Retrieval-Augmented Generation

- Enhancing Large Language Models with external knowledge
- Accessing external knowledge bases
- Retrieval and semantic search
- Grounded response generation
- Applications in chatbots and knowledge-base enrichment

### 14. State-of-the-Art NLP Applications

- Text summarisation
- Machine translation
- Question answering
- Conversational systems
- Other emerging NLP applications

---

## 16-Topic Learning Path

| # | Topic | Main Focus | Reference |
|---:|---|---|---|
| 1 | Natural Language Understanding and Generation | Language study, NLU applications, evaluation, levels of analysis, organisation of NLP systems | T2 and supporting resources |
| 2 | Vector Semantics and Embeddings | Lexical and vector semantics, TF-IDF, Word2Vec, Skip-gram, CBOW, GloVe, embedding visualisation | T1 and notes |
| 3 | N-gram Language Modelling | N-grams, generalisation, zero probabilities, smoothing, Stupid Backoff, model evaluation | T1 Ch. 3 |
| 4 | Neural Networks and Neural Language Modelling | Feed-forward networks, training, neural language models | R2 Ch. 4 |
| 5 | Introduction to LLMs and Prompt Engineering | LLM foundations, zero-shot and few-shot prompting, chain-of-thought, generated knowledge | Research papers and supporting resources |
| 6 | Part-of-Speech Tagging | Word classes, Penn Treebank tags, Markov Chains, HMMs, HMM tagging | T1 Ch. 8 |
| 7 | Statistical, ML, and Neural Models for POS Tagging | Forward and Viterbi algorithms, Maximum Entropy Markov Models, bidirectionality, neural models | T1 Appendix A and research papers |
| 8 | Foundations Review | Consolidation of language understanding, representation, modelling, and tagging | Topics 1–7 |
| 9 | Constituency Parsing | Grammars, Bottom-up Chart Parsing, PCFGs, CKY parsing, lexicalised CFGs | T2 Ch. 3; T1 Ch. 14 |
| 10 | Dependency Parsing | Dependency relations, treebanks, transition-based, graph-based, and neural parsing | T1 Ch. 19 |
| 11 | Encoder-Decoder Models, Attention, and Contextual Embeddings | Encoder-decoder networks, attention, self-attention, transformers, BERT, contextual representations | T1 Ch. 10 |
| 12 | Word Sense Disambiguation | Word senses, lexical relations, WordNet, WSD algorithms, Word Sense Induction | T1 Ch. 15 |
| 13 | Semantic Web, Ontologies, and Knowledge Graphs | Ontologies, ontology engineering and learning, knowledge-graph construction | R1 Ch. 24 and research papers |
| 14 | Retrieval-Augmented Generation | External knowledge, semantic search, retrieval, grounded generation, chatbot enrichment | Research papers and supporting resources |
| 15 | State-of-the-Art Applications | Text summarisation and other modern NLP applications | Research papers |
| 16 | Advanced Topics Review | Consolidation of parsing, contextual models, semantics, knowledge graphs, RAG, and applications | Topics 9–15 |

---

## How the Main NLP Layers Fit Together

| Layer | Main Question | Typical Techniques |
|---|---|---|
| Lexical | What does a word represent? | TF-IDF, embeddings, WordNet |
| Syntactic | How are words structurally related? | POS tagging, constituency parsing, dependency parsing |
| Semantic | What does the text mean? | Vector semantics, WSD, contextual embeddings |
| Knowledge | What external facts and relationships are relevant? | Ontologies, knowledge graphs, retrieval |
| Generation | How can meaningful language be produced? | Language models, encoder-decoder models, transformers, RAG |

---

## Experiments

| # | Experiment | Related Topic |
|---:|---|---:|
| 1 | Explore NLTK, spaCy, and other NLP tools | 1 |
| 2 | Implement word embeddings using Skip-gram and CBOW | 2 |
| 3 | Build an N-gram language model | 3 |
| 4 | Build a neural language model | 4 |
| 5 | Implement Part-of-Speech tagging | 6–7 |
| 6 | Implement constituency and dependency parsing | 9–10 |
| 7 | Explore BERT and Large Language Models | 5, 11 |
| 8 | Work with WordNet, ontologies, and knowledge graphs | 12–13 |
| 9 | Build a Retrieval-Augmented Generation pipeline | 14 |
| 10 | Implement text summarisation | 15 |

### Practical Tools

- **Programming language:** Python
- **NLP libraries:** NLTK, spaCy, Hugging Face Transformers
- **Machine-learning libraries:** Scikit-learn, TensorFlow
- **Knowledge-graph tools:** Neo4j
- **LLM tools:** OpenAI APIs and open-source models
- **Environment:** Jupyter Notebook and Google Colab

---

## Book References

### Primary Textbooks

1. Daniel Jurafsky and James H. Martin, *Speech and Language Processing: An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition*.
2. Christopher D. Manning and Hinrich Schütze, *Foundations of Statistical Natural Language Processing*, MIT Press.

### Reference Books

1. James Allen, *Natural Language Understanding*.
2. Philipp Koehn, *Neural Machine Translation*.

---

## Key Takeaways

{{% hint success %}}
- NLP connects linguistic structure with computational models of language.
- Vector representations and language models provide the foundation for modern NLP systems.
- Tagging and parsing reveal grammatical structure, while semantic methods focus on meaning.
- Attention, transformers, and contextual embeddings enable powerful language understanding and generation.
- Ontologies, knowledge graphs, and retrieval connect language models to structured and external knowledge.
- RAG combines retrieval with generation to produce more informed and grounded responses.
{{% /hint %}}

---

{{< home-link "Home" >}} | {{< section-index >}}
