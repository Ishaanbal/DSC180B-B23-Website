# Predicting Unknown Side Effects of the Cancer Drug Pralsetinib
**A Neuro-Symbolic Approach to Drug Safety using Knowledge Graphs**

**Team Members:** [Insert Your Names Here]

---

## Abstract
Pralsetinib has limited long-term real-world safety data. Post-marketing pharmacovigilance already shows unexpected adverse effects beyond on-target effects. Because it is new, used in a small subset of patients, and has complex kinase biology, there is a high "unknown off-target space"—making it well-suited for an ontology-based off-target prediction study. Our approach builds a neuro-symbolic knowledge graph linking mechanisms to toxicity, enriching it with ontologies, and trains a Graph Neural Network to predict specific off-target adverse outcomes.

## Introduction: The Problem
* **The Hook:** Pralsetinib is a recently approved, powerful cancer drug (a RET kinase inhibitor). However, because it is so new, we don't have enough long-term safety data.
* **The Problem:** Post-marketing data is showing unexpected adverse side effects like severe infections, cognitive disorders, and rhabdomyolysis. These happen because the drug accidentally interacts with "off-target" proteins in the body.
* **Our Goal:** Predict these unknown off-target effects before they surprise patients and doctors, creating a safety map for Pralsetinib.

## Methods
We built a **Knowledge Graph**—a giant web connecting drugs, proteins, diseases, and adverse events based on decades of biomedical literature and chemical data. Then, we trained an AI (a Graph Neural Network) to look at this web and "guess" where missing connections should be.

<details>
  <summary><strong>Click here for the technical details</strong></summary>
  <p>We extracted data from PubChem, ChEMBL, and OpenTargets to build a base graph. We enriched it with Gene Ontology (GO) pathways and expanded it using STRING and UniProt. Finally, we trained a Graph Neural Network (GNN) on known <code>(drug, inhibits, protein)</code> and <code>(protein, associated_with, Disease)</code> edges to rank novel off-target proteins and predict specific adverse outcomes.</p>
</details>

## Results
* **High-Level Explanation:** Our model was highly successful at finding the "needle in the haystack." When we hid known interactions from the AI to test it, our model successfully rediscovered 100% of those known drug-protein targets within its top 20 predictions!
* **Beating the Baseline:** It significantly outperformed simpler prediction models because it was able to look at the *multi-hop structure* of the graph, rather than just direct connections.

## Conclusion
Our final deliverable is a ranked list of predicted adverse effects tied to specific off-target proteins. This provides pharmacologists and doctors with concrete, data-backed hypotheses for safety-relevant outcomes. By using AI to flag these risks early, we can help keep patients safer and guide better clinical monitoring for newly approved drugs.

---
### Important Links
* [GitHub Code Repository](https://github.com/Ishaanbal/DSC180B-B23-Knowledge-Graph-and-Biomedical-Ontology)
* [Full Project Report](#) *(Link your PDF here later)*
