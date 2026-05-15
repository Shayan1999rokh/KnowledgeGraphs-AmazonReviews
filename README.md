# Amazon Review Knowledge Graph using NLP and Dependency Parsing

## Overview

This project focuses on extracting structured knowledge from unstructured customer reviews in the Amazon Cell Phones and Accessories Reviews dataset using Natural Language Processing (NLP) techniques.

The system automatically processes review text, identifies entities and relationships using dependency parsing, and constructs a knowledge graph representing interactions, opinions, and semantic relationships found within customer reviews.

The extracted information is transformed into structured triples:

```text
(Entity 1) → (Relation) → (Entity 2)
```

These triples are then used to build a directed knowledge graph where:

* Entities become nodes
* Relations become edges

Finally, the graph is visualized using graph analysis libraries to reveal patterns and relationships hidden inside large-scale review data.


---

# Project Objectives

The main goals of this project are:

* Process raw Amazon review text
* Perform text preprocessing and tokenization
* Apply dependency parsing using spaCy
* Extract entities and semantic relations
* Generate structured knowledge triples
* Construct a knowledge graph
* Visualize entity relationships
* Analyze customer opinions and dependencies

---

# Technologies Used

| Technology | Purpose                    |
| ---------- | -------------------------- |
| Python     | Main programming language  |
| pandas     | Data processing            |
| spaCy      | NLP and dependency parsing |
| NetworkX   | Knowledge graph creation   |
| Matplotlib | Graph plotting             |
| gensim     | Text preprocessing         |
| tqdm       | Progress visualization     |

---

# Dataset

The project uses the Amazon Cell Phones and Accessories review dataset.

## Dataset Characteristics

| Property     | Description                |
| ------------ | -------------------------- |
| Dataset Type | Product reviews            |
| Domain       | Cell phones & accessories  |
| Format       | JSON                       |
| Data Source  | Amazon reviews             |
| Content      | User-generated review text |

---

# Project Pipeline

The workflow consists of the following stages:

```text
Raw Reviews
      ↓
Text Preprocessing
      ↓
Dependency Parsing
      ↓
Entity Extraction
      ↓
Relation Extraction
      ↓
Triple Generation
      ↓
Knowledge Graph Construction
      ↓
Visualization & Analysis
```

---

# Required Libraries

Install all required dependencies before running the project.

## Installation

```bash
pip install pandas gensim spacy networkx matplotlib tqdm beautifulsoup4 requests
```

---

# Download spaCy English Model

```bash
python -m spacy download en_core_web_sm
```

---

# Import Libraries

```python
import pandas as pd
import json
import gensim
import re
import bs4
import requests
import spacy
import networkx as nx
import matplotlib.pyplot as plt

from tqdm import tqdm
from spacy.matcher import Matcher
from spacy.tokens import Span
from spacy import displacy
```

---

# Loading Dataset

```python
df = pd.read_json(
    "/kaggle/input/amazon-reviews/Cell_Phones_and_Accessories_5.json",
    lines=True
)
```

The dataset is loaded directly into a Pandas DataFrame for efficient manipulation and analysis.

---

# Text Preprocessing

The review text is preprocessed using Gensim utilities.

```python
review_text = df['reviewText'].apply(
    gensim.utils.simple_preprocess
)
```

---

# Preprocessing Operations

The preprocessing stage includes:

* Lowercasing text
* Tokenization
* Removing punctuation
* Cleaning noisy symbols
* Standardizing words

---

# Dependency Parsing with spaCy

The project uses the spaCy English model for dependency parsing.

```python
nlp = spacy.load('en_core_web_sm')
```

Dependency parsing identifies grammatical relationships between words.

---

# Example Dependency Parsing

Input sentence:

```text
the drawdown process is governed by astm standard d823
```

Parsed dependencies:

```text
drawdown → modifier
process → subject
governed → ROOT verb
standard → object
```

This syntactic structure helps identify meaningful entity relationships.

---

# Entity Extraction

The project extracts:

* Subjects
* Objects

from each review sentence.

---

# Entity Extraction Function

```python
def get_entities(sent):
```

The algorithm:

* Identifies compound words
* Detects modifiers
* Extracts grammatical subjects
* Extracts grammatical objects

---

# Example Entity Extraction

Input review:

```text
These stickers work like the review says they do.
```

Extracted entities:

```text
Entity 1: stickers
Entity 2: review
```

---

# Relation Extraction

The project extracts semantic relations between entities using dependency patterns.

---

# Relation Extraction Function

```python
def get_relation(sent):
```

The matcher identifies patterns such as:

```text
ROOT verb
+ optional preposition
+ optional adjective
```

---

# Example Relation Extraction

Input:

```text
I need a charger
```

Extracted triple:

```text
(I) → (need) → (charger)
```

---

# Triple Construction

The extracted information is stored as structured triples:

| Source   | Relation  | Target  |
| -------- | --------- | ------- |
| user     | need      | charger |
| phone    | has       | battery |
| customer | recommend | product |

---

# Knowledge Graph DataFrame

```python
kg_df = pd.DataFrame({
    'source': source,
    'target': target,
    'edge': relations
})
```

---

# Knowledge Graph Construction

The graph is created using NetworkX.

```python
G = nx.from_pandas_edgelist(
    kg_df,
    "source",
    "target",
    edge_attr=True,
    create_using=nx.MultiDiGraph()
)
```

---

# Directed Graph Representation

The graph is represented as:

```text
Node → Relation → Node
```

Where:

* Nodes represent entities
* Directed edges represent semantic relationships

---

# Graph Visualization

The knowledge graph is visualized using:

```python
nx.draw()
```

with:

* Spring layout positioning
* Colored nodes
* Directed edges
* Relationship structure

---

# Full Knowledge Graph

The full graph reveals:

* Product interactions
* User opinions
* Frequently occurring concepts
* Semantic dependencies

---

# Relation Frequency Analysis

The most common extracted relations include:

| Relation  | Frequency |
| --------- | --------- |
| is        | 33        |
| recommend | 24        |
| buy       | 10        |
| need      | 6         |
| works     | 6         |
| love      | 5         |
| charges   | 5         |

These relations reflect common customer behaviors and sentiments.

---

# Focused Subgraph Analysis

The project also generates focused subgraphs for specific relations.

Example:

```python
kg_df[kg_df['edge'] == "need"]
```

This isolates all:

* “need” relationships
* dependency patterns
* common customer requirements

---

# Need-Relation Graph

The “need” subgraph highlights:

* Frequently requested products
* Common accessory dependencies
* Customer expectations

This provides valuable business insights.

---

# Example Workflow

## Input Review

```text
I need a better charger for my phone.
```

## Extracted Entities

```text
Entity 1: I
Entity 2: charger
```

## Extracted Relation

```text
need
```

## Generated Triple

```text
(I) → (need) → (charger)
```

---

# Visualization Example

The graph visualization reveals:

* Dense semantic connections
* Central entities
* Frequently mentioned products
* Customer-product relationships

---

# Applications

This project has applications in:

* Recommendation systems
* Opinion mining
* Semantic search
* Knowledge graph generation
* E-commerce analytics
* Customer behavior analysis
* Product dependency analysis
* Review summarization

---

# Advantages of the Approach

## Structured Knowledge Extraction

Transforms raw text into machine-readable knowledge.

## Unsupervised Information Mining

No manual annotation required.

## Semantic Relationship Discovery

Captures meaningful grammatical relationships.

## Graph-Based Representation

Enables visual and structural analysis.

---

# Limitations

## Dependency Parsing Errors

Informal reviews may contain:

* grammar mistakes
* abbreviations
* incomplete sentences

which reduce extraction quality.

## Entity Ambiguity

Different words may refer to the same concept.

Example:

```text
phone
mobile
device
```

## Small Subset Processing

Only the first 500 reviews are analyzed in this implementation.

---

# Possible Improvements

## Named Entity Recognition (NER)

Improve entity quality using advanced entity extraction.

## Lemmatization

Normalize words into base forms.

Example:

```text
buying → buy
```

## Transformer Models

Use transformer-based NLP models such as:

* BERT
* RoBERTa
* GPT

## Sentiment Analysis Integration

Combine:

* entities
* relations
* sentiment polarity

for richer knowledge graphs.

## Larger Dataset Processing

Scale the pipeline to millions of reviews.

## Interactive Visualization

Use:

* PyVis
* Plotly
* Gephi

for interactive exploration.

---

# Running the Project

## Execute the Notebook

Run the notebook cells sequentially.

Or execute:

```bash
python main.py
```

---

# Expected Outputs

The project generates:

| Output                  | Description               |
| ----------------------- | ------------------------- |
| Extracted entities      | Subject-object pairs      |
| Extracted relations     | Verb relationships        |
| Knowledge graph         | Structured semantic graph |
| Relation statistics     | Frequency analysis        |
| Subgraph visualizations | Focused relation analysis |

---

# Example Knowledge Triple

```text
(customer) → (recommend) → (product)
```

---

# Graph Interpretation

The graph structure helps identify:

* Popular accessories
* User preferences
* Common complaints
* Product dependencies
* Frequently discussed features

---

# Conclusion

This project demonstrates how Natural Language Processing and dependency parsing can transform unstructured Amazon reviews into structured semantic knowledge graphs.

By combining:

* text preprocessing
* entity extraction
* relation extraction
* graph construction

the system enables deeper analysis of customer opinions and product relationships.

The project provides a practical foundation for:

* knowledge graph construction
* semantic mining
* NLP-based information extraction
* graph-based analytics in e-commerce systems

---
# !!! ReadMe was produced by GPT. Check the important info
* [Gensim Documentation](https://radimrehurek.com/gensim/?utm_source=chatgpt.com)
* [Matplotlib Documentation](https://matplotlib.org/stable/contents.html?utm_source=chatgpt.com)
