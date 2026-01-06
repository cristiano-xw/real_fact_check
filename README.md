
# Multi-Hop Fact-Checking 

This repository contains a specialized dataset designed for **multi-hop fact-checking**, **temporal reasoning**, and **hallucination detection**. The data is organized into two primary components: raw real-world claims scraped from fact-checking authorities, and a synthetic multi-hop dataset featuring diverse negative sampling strategies.

## 📂 Data Files

### 1. `raw_data_example.jsonl`
**Description:** Contains raw fact-checking data collected from professional verification websites (e.g., Snopes, PolitiFact). This file serves as the ground truth source for real-world events and evidences.

**Field Definitions:**

| Field | Type | Description |
| :--- | :--- | :--- |
| `claim` | `str` | The statement or claim being verified. |
| `label` | `str` | The verdict (e.g., "False", "True", "Miscaptioned"). |
| `evidence` | `str` | The full text content/report from the fact-checking article. |
| `publishdate` | `str` | The publication date of the fact-check (YYYY-MM-DD). |
| `source` | `str` | Citations, URLs, and metadata regarding the source of the claim. |
| `tag` | `list` | Keywords or entities associated with the claim (e.g., ["India", "Turkey"]). |
| `time_sensitive` | `int` | Binary flag (`1` or `0`) indicating if temporal context is crucial for verification. |

---



### 2. `multi-hop_samples.jsonl`

**Description:** A complex reasoning dataset where each entry contains one `true_claim` and multiple `negative_samples` (Hard Negatives) generated via specific perturbation strategies.

#### **Top-Level Structure**

| Field | Type | Description |
| --- | --- | --- |
| `hop_count` | `int` | Total reasoning steps required for verification. |
| `source_entities` | `list` | Core entities involved in the multi-hop chain. |
| `true_claim` | `obj` | The factually correct reasoning object. |
| `negative_samples` | `list` | List of corrupted claims for hallucination testing. |

#### **A. The `true_claim` & `negative_samples` Object Detail**

Both objects share a similar internal structure to facilitate direct comparison:

| Field | Description |
| --- | --- |
| `claim` | The full statement (True or Perturbed). |
| `claim_date` | The temporal anchor for the claim. |
| `label` | "True" for the gold sample; "False" for negative samples. |
| `main_evidence` | List of atomic evidence snippets with associated dates. |
| `explanation` | A natural language summary of the logical/temporal reasoning chain. |
| `sub_claims` | **Step-by-step decomposition** featuring `step`, `question`, `answer`, `evidence`, and `date` for each hop. |
| `strategy` | *(Negative samples only)* The perturbation method used (see below). |

---

## 🛠 Perturbation Strategies

| Strategy | Logic |
| --- | --- |
| **Entity Hallucination** | Swaps key entities (e.g., swapping "Indiana Fever" for "Chicago Sky"). |
| **Chronological Distortion** | Shifts dates to break temporal causality or incumbency. |
| **Relational Corruption** | Corrupts the link between entities (e.g., placing a Japanese landmark in Uganda). |
| **Contextual Contradiction** | Retains facts but contradicts the established legal or logical context. |
| **Data Perturbation** | Subtle modification of numerical values (e.g., changing salary or population figures). |
| **Sentiment Inversion** | Negates a core factual assertion within the chain (e.g., "was" to "was not"). |

---


