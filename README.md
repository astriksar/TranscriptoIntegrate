# Transcriptomic Integration Strategies

This document summarizes common strategies for integrating transcriptomic data across studies, platforms, and systems. The strategies range from low-level data merging to high-level model abstraction.

---

## 1️⃣ Data Integration (Technical)

**Process:**  
Raw data or normalized counts from multiple studies, platforms (RNA-seq, Microarray, Agilent), and species/systems (Human, Mouse) are merged into a common data space.

**Typical Methods:**  
- ComBat / sva  
- RUV / Quantile Normalization  
- Cross-platform scaling

**Challenges:**  
- Batch effects are **not** equivalent to biological effects.  
- Methodologically often misleading in multi-system toxicology studies.  

**Takeaway:**  
Only reliable if study designs are reasonably comparable.

**References:**  

- Andrey A. Shabalin, Håkon Tjelmeland, Cheng Fan, Charles M. Perou, Andrew B. Nobel, Merging two gene-expression studies via cross-platform normalization, Bioinformatics, Volume 24, Issue 9, May 2008, Pages 1154–1160, [LINK](https://academic.oup.com/bioinformatics/article/24/9/1154/206630?login=true)
    *  XPN (cross-platform normalization) method
    * XPN input: the gene-expression measurements from two (or more) studies →
    * uses block-clustering approach (K-Means Clustering)
    * Data sets:
      1. Affymetrix GeneChip U95Av2 array
      2. 25K Agilent oligonucleotide array
      3. 22K Agilent oligonucleotide array

- Johnson, W. E., Li, C., & Rabinovic, A. (2007). *Adjusting batch effects in microarray expression data using empirical Bayes methods.* Biostatistics. [LINK](https://pubmed.ncbi.nlm.nih.gov/16632515)


---

## 2️⃣ Result Integration (Meta-Analysis) 

> [!NOTE] KINDA WHAT WE DID SO FAR

**Process:**  
Each study is analyzed independently. Integration happens at the level of:  
- Differentially Expressed Genes (DEGs)  
- Ranks  
- Effect Sizes

**Examples of Methods:**  
- Rank aggregation (e.g., RobustRankAggreg)  
- Fisher / Stouffer Meta p-values  
- Vote counting across studies

**Advantages:**  
- Robust against platform and system differences.  
- Ideal for identifying conserved cross-study signatures.

**Takeaway:**  
Methodologically sound approach for multi-study comparisons.

**References:**  
- Ramasamy, A., Mondry, A., Holmes, C. C., & Altman, D. G. (2008). *Key Issues in Conducting a Meta-Analysis of Gene Expression Microarray Datasets.* PLoS Med., 5(9), e184. [Link](https://doi.org/10.1371/journal.pmed.0050184)  
- Beccacece et al., 2023 Cross-Species Transcriptomics Analysis Highlights Conserved Molecular Responses to Per- and Polyfluoroalkyl Substances [LINK](https://pmc.ncbi.nlm.nih.gov/articles/PMC10385990/)


  * Transcriptome-wide signature formula: <mark> −log10(p) × sign(log(FC))</mark>
  * Ortholog Prediction 
      → cross-species comparison via common gene identifiers
  * Signature Analysis
      → NES integration: Stouffer method via corto package.
  * Metabolite Prediction
      → Correlation-based prediction using Cancer Cell Line Encyclopedia metabolomics-transcriptomics data


---

## 3️⃣ Pathway / Signature Integration

**Process:**  
- Instead of comparing genes directly, map genes to functional units (pathways/signatures).  
- Examples: KEGG, Reactome, Stress, Apoptosis, PP2A, ER Stress signatures.  

**Integration Approach:**  
- Identify conserved pathways and consistent direction of regulation across studies and systems.

**Why it Works:**  
- Gene-level variation is high, but biological programs are more stable.  

**Takeaway:**  
- Often the strongest evidence line in toxicogenomic analyses.

---

## 4️⃣ Model Integration (Highest Abstraction)

**Process:**  
- Train ML models or compute scores per study.  
- Make models/scores comparable across studies.

**Examples:**  
- Signature scores  
- GSVA / ssGSEA  
- Classifier transfer across datasets

**Result:**  
- Enables statements such as:  
  > "OA induces a conserved transcriptional stress response"  
- Does **not** require identical genes across studies.

---

## Summary

| Strategy                  | Level            | Pros                                      | Cons / Notes                                     |
|----------------------------|----------------|------------------------------------------|------------------------------------------------|
| Data Integration           | Technical       | Unified dataset for downstream analysis | Sensitive to batch effects, requires similar study designs |
| Result Integration         | Meta-analysis   | Robust across platforms & species        | Loses some granularity of raw data            |
| Pathway / Signature        | Functional      | Stable biological interpretation         | Dependent on curated pathways/signatures      |
| Model Integration          | Abstract/ML     | High-level biological conclusions        | Requires modeling expertise                    |

---
