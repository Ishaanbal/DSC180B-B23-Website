---
    layout: default
    title: Home
    ---
    
# Predicting Unknown Side Effects of the Cancer Drug Pralsetinib
    **A Neuro-Symbolic Approach to Drug Safety using Knowledge Graphs**
    
    **Team Members:** Ryan Cao, Suchit Bhayani, Ishaan Bal, Taranvir Chima
    
    ---
    
    ## Abstract
    We study Pralsetinib off-target effects using a neuro-symbolic pipeline centered on a knowledge graph (KG). Our goal is to replace a drug-centric star topology with a mechanistic, multi-hop structure that supports interpretable reasoning (Drug → Protein → Outcome). We construct a baseline KG from PubChem Bioactivity, indications, clinical trials, literature mining, and co-occurrence signals, then enrich it with Gene Ontology (GO) biological processes and CTD protein-disease associations. This adds lateral Protein-Pathway and Protein-Outcome links while controlling node explosion. We implement a baseline GNN for link prediction and run a small held-out evaluation; results show strong recall of known targets but limited evidence of novel discovery, largely due to the small positive set.
    
    ## Introduction: The Problem
    **The Hook:** Modern pharma R&D is constrained by fragmented, heterogeneous evidence. Assays, literature, clinical reports, and observational data rarely align in a single structured representation.
    **The Problem:** While LLMs can extract entities from text, they do not reliably enforce consistency, provenance, or multi-hop mechanistic structure. Pralsetinib, a recently approved cancer drug, needs better mapping of its off-target effects to predict unexpected adverse side effects.
    **Our Goal:** We shift from text-only extraction to a graph-based representation. By building a Knowledge Graph (KG) that encodes explicit relationships, we enable auditable reasoning and reproducible off-target hypotheses.
    
    ## Methods
    ### 1. Knowledge Graph Construction
    We constructed the baseline KG from PubChem-derived sources:
    * **Bioactivity assays** (direct drug-target evidence mapping to `inhibits` edges)
    * **Clinical trials and indications** (mapping to `treats` edges)
    * **Literature mining** (adverse event text mining mapping to `associated_with` edges)
    * **Chemical/gene co-occurrence signals** (weak evidence links)
    
    <details>
    <summary><strong>Click here for the technical enrichment details</strong></summary>
    <p>To move beyond a simple "star topology" centered on the drug, we enriched the KG using Biomedical Ontologies:</p>
    <ul>
    <li><strong>Gene Ontology (GO):</strong> We queried GO via the UniProt API to link proteins to Biological Processes, adding `involved_in` edges and Pathway nodes.</li>
    <li><strong>Comparative Toxicogenomics Database (CTD):</strong> We integrated CTD data to link proteins to Disease outcomes, adding `implicated_in` edges and Disease nodes.</li>
    </ul>
    <p>This enrichment adds lateral Protein-Pathway and Protein-Disease connections, moving beyond the drug-centric view and enabling multi-hop reasoning while controlling node set expansion.</p>
    </details>
    
    ### 2. KG Visualization
    The enriched KG composition shows the distribution of node and edge types:
    ![Enriched KG Composition](images/KG_composition.png)
    
    A subset of the KG illustrates the connections:
    ![KG Subset Visualization](images/KG_subset.png)
    
    ### 3. Baseline vs. GNN for Link Prediction
    We implemented a baseline model and a Graph Neural Network (GNN) for link prediction (predicting `inhibits` edges between Drug and Protein nodes).
    * **Baseline:** Simple prediction based on co-occurrence or known associations.
    * **GNN (GraphSAGE):** Learns node embeddings from the graph structure to predict links.
    
    The GNN was trained on a subset of known `inhibits` links and evaluated on a held-out set.
    
    ## Results
    The GNN model showed promising results in recalling known drug-target interactions within the test set. However, predicting entirely novel off-target interactions proved challenging given the limited size of the positive training set (known off-targets).
    ![Target Predictions](images/target_predictions.png)
    
    ## Discussion
    Our neuro-symbolic pipeline successfully constructed and enriched a KG for Pralsetinib. The GNN model demonstrates the potential of graph-based learning for predicting drug-target interactions. Future work should focus on expanding the training data and incorporating more diverse biological evidence to improve novel link prediction and better understand off-target effects.
    
    ## Conclusion
    We developed a KG-centered approach to model Pralsetinib's interactions, moving towards more interpretable and mechanistic predictions of off-target effects compared to purely text-based methods.
    
    ---
    
