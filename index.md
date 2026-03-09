---
layout: default
title: Home
---

# Predicting Unknown Side Effects of the Cancer Drug Pralsetinib
**A Neuro-Symbolic Approach to Drug Safety using Knowledge Graphs**

**Team Members:** Ryan Cao, Suchit Bhayani, Ishaan Bal, Taranvir Chima

---

## Abstract
We study Pralsetinib off-target effects using a neuro-symbolic pipeline centered on a knowledge graph (KG). Our goal is to replace a drug-centric star topology with a mechanistic, multi-hop structure that supports interpretable reasoning (Drug → Protein → Outcome). We construct a baseline KG from PubChem bioactivity, indications, clinical trials, literature mining, and co-occurrence signals, then enrich it with Gene Ontology (GO) biological processes and CTD protein-disease associations. This adds lateral Protein-Pathway and Protein-Outcome links while controlling node explosion. We implement a baseline GNN for link prediction and run a small held-out evaluation; results show strong recall of known targets but limited evidence of novel discovery, largely due to the small positive set. 

## Introduction: The Problem
* **The Hook:** Modern pharma R&D is constrained by fragmented, heterogeneous evidence. Assays, literature, clinical reports, and observational data rarely align in a single structured representation.
* **The Problem:** While LLMs can extract entities from text, they do not reliably enforce consistency, provenance, or multi-hop mechanistic structure. Pralsetinib, a recently approved cancer drug, needs better mapping of its off-target effects to predict unexpected adverse side effects.
* **Our Goal:** We shift from text-only extraction to a graph-based representation. By building a Knowledge Graph (KG) that encodes explicit relationships, we enable auditable reasoning and reproducible off-target hypotheses.

## Methods

### 1. Knowledge Graph Construction
![KG Composition](https://raw.githubusercontent.com/Ishaanbal/DSC180B-B23-Website/main/images/KG_composition.png)

![KG Subset](https://raw.githubusercontent.com/Ishaanbal/DSC180B-B23-Website/main/images/KG_subset.png)
We constructed the baseline KG from PubChem-derived sources:
* **Bioactivity assays** (direct drug-target evidence mapping to `inhibits` edges)
* **Clinical trials and indications** (mapping to `treats` edges)
* **Literature mining** (adverse event text mining mapping to `associated_with` edges)
* **Chemical/gene co-occurrence signals** (weak evidence links)

<details>
  <summary><strong>Click here for the technical enrichment details</strong></summary>
  <p>To move beyond a simple "star topology" centered on the drug, we enriched the KG using Biomedical Ontologies:</p>
  <ul>
    <li><strong>Gene Ontology (GO):</strong> We queried GO via the UniProt API to link proteins to Biological Processes, adding <code>involved_in</code> edges and Pathway nodes.</li>
    <li><strong>Comparative Toxicogenomics Database (CTD):</strong> We used CTD to connect proteins to clinical outcomes (Disease/AE) via <code>associated_with</code> edges, focusing on oncology to prevent node explosion.</li>
  </ul>
  <p>After enrichment, the graph grew to 580 nodes and 1,456 edges, with proteins and diseases dominating the structure.</p>
</details>

### 2. Modeling
We compared a simple KG baseline against a Graph Neural Network (GNN) link-prediction model:
* **Baseline:** Ranks proteins using only direct drug-protein edges and their numeric values (e.g., IC50 sum).
* **GNN:** Uses the full KG, learns node embeddings with a 2-layer GCN, and scores links with an MLP head. It exploits multi-hop structure and shared connectivity patterns.

![Target Predictions](https://raw.githubusercontent.com/Ishaanbal/DSC180B-B23-Website/main/images/target_predictions.png)

## Results
* **High-Level Explanation:** When evaluated on known targets, our GNN model successfully recovered 100% of the known drug-protein targets within its top 20 predictions!
* **Beating the Baseline:** The GNN retrieved significantly more known targets at larger *k* (Top-5, Top-10, Top-20) compared to the baseline model, highlighting the value of multi-hop graph structure over direct evidence alone. 
* *Note:* Performance is currently constrained by the limited number of known positives (13 total), so expanding the protein set is a focus for future robust evaluation.

## Conclusion
Our neuro-symbolic pipeline demonstrates that combining structured assay evidence with ontology grounding (GO/CTD) enables mechanistic multi-hop reasoning. The final deliverable provides pharmacologists with transparent, data-backed hypotheses for Pralsetinib's safety-relevant outcomes, addressing the critical interpretability challenges in AI-driven drug discovery.

---
### Important Links
* [GitHub Code Repository](https://github.com/Ishaanbal/DSC180B-B23-Knowledge-Graph-and-Biomedical-Ontology)
