# MDCite  
**A Large-Scale Multi-Disciplinary Citation Context and Intent Dataset**

MDCite is a large-scale, multi-disciplinary citation context dataset designed to support research in citation-aware scholarly information retrieval (IR).  
The dataset treats citation contexts local textual spans surrounding in-text citations as the primary unit of retrieval and analysis, enabling fine-grained evaluation of intent-aware retrieval, ranking, and candidate generation methods.

This repository accompanies the **SIGIR 2026 Resource Paper** introducing MDCite and provides all artifacts required to reproduce the dataset and its construction pipeline.

---

##  Overview

- **Citation contexts:** 2,057,196  
- **Citing papers:** ~100,000  
- **Scientific fields:** 21  
- **Publication venues:** >1,000  
- **Citation intent classes:** 7  
- **Time span:** 2000–2024  

MDCite is designed as a **realistic IR test collection**, preserving the scale, imbalance, and disciplinary heterogeneity of real-world scholarly citations.

---

##  Data Sources

MDCite is constructed by integrating multiple large-scale scholarly data sources:

### Scopus bibliographic records (2000–2024)
Bibliographic metadata are collected via the **Scopus API**, providing access to journal articles, citation counts, and rich publication metadata.  
These records are used to identify influential papers based on citation statistics and support reproducible large-scale data collection over a long temporal span.

### Web of Science (WoS) 2024 Subject Categories
WoS 2024 subject categories are used to group journals by scientific field and select **Top-5 Q1 journals per field**.  
This field-aware normalization controls for substantial differences in citation practices across disciplines.

### OpenAlex API
The **OpenAlex API** is used to extract citation links and reference-linked citation context spans from full-text scholarly works that cite the selected influential papers.  
This enables large-scale extraction of citation contexts suitable for retrieval and intent analysis.

---

##  Dataset Construction Pipeline

The MDCite dataset is built through a transparent and reproducible pipeline:

1. **Journal selection**
   - Journals grouped by WoS subject categories  
   - Top-5 Q1 journals selected per field  

2. **Citation-based filtering**
   - Top 5% most-cited papers identified independently within each journal  
   - Prevents dominance by citation-intensive fields  

3. **Citation context extraction**
   - Citation contexts extracted from papers citing the selected influential papers  
   - Each context corresponds to a textual span surrounding an in-text citation marker  

4. **Citation intent classification**
   - Each context is automatically assigned one of seven intents:  
     `background`, `uses`, `similarities`, `differences`, `motivation`, `extends`, `future_work`

5. **Dataset variants**
   - Multi-intent intermediate artifacts (for provenance)  
   - Single-intent benchmark variant (for standard IR and classification)

---

##  File Structure

### Core Dataset Files

- **`dataset_context_intent_single.csv`** (1.45 GB)  
  Single-intent benchmark version in CSV format  
  - One row = one citation context  
  - Broad compatibility with IR toolchains  

- **`dataset_context_intent_single.parquet`** (620 MB)  
  Same content in Parquet format  
  - Recommended for large-scale indexing and retrieval  

- **`citation_context_intent_data.zip`** (2.29 GB)  
  Full citation context and multi-intent data  
  - Includes intermediate construction artifacts  

### Construction & Provenance Files

- **`Scopus_(Year_2000–2024).zip`** (120 MB)  
  Bibliographic metadata collected via Scopus API  

- **`WOS_2024_Subject_categories_(Top-5_Q1_Journals_per_field)_data.zip`** (51 MB)  
  Journal lists grouped by scientific field  

- **`Top_5%_cited_papers_per_journal_dataset.zip`** (8.05 MB)  
  Lists of Top-5% cited papers selected per journal  

---

##  Dataset Schema

Each citation context instance contains the following fields:

| Field | Description |
|------|------------|
| `text` | Citation context text |
| `label` | Functional citation intent |
| `field` | Scientific field |
| `group_id` | Citing paper identifier |
| `paperId` | OpenAlex work identifier |
| `doi` | Digital Object Identifier |
| `venue` | Publication venue |
| `year` | Publication year |
| `source_file` | Provenance identifier |

The `group_id` field enables **document-aware evaluation** and prevents information leakage in retrieval experiments.

---

##  Intent Distribution

MDCite contains seven citation intent classes and exhibits a highly imbalanced distribution, reflecting real-world citation behavior rather than artificially balanced labels.

---

##  Intended Use Cases

MDCite supports a wide range of research scenarios, including:

- Citation-aware information retrieval  
- Intent-aware ranking and re-ranking  
- Candidate generation analysis  
- Large-scale citation intent classification  
- Scholarly search and citation analysis  

Baseline BM25 experiments demonstrate that MDCite naturally supports deep-retrieval evaluation, where recall improves only at larger cutoffs due to large and distributed relevance sets.

---

##  Reproducibility 

All files are released for research and educational use.  
MDCite is designed to support end-to-end reproducible experimentation, with clear provenance, intermediate artifacts, and document-level identifiers to avoid information leakage.

---

##  Data Availability

The processed citation context and intent datasets used in this study are
publicly available via Zenodo at https://doi.org/10.5281/zenodo.18410050.

---
