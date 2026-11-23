# Datasets — the working decision

*Decision taken 2026-11-22. Full search record with every candidate checked: `DATASETS-SEARCH.md`.*

**Primary dataset: GSE73754.** 

| Role | Dataset | Why |
|---|---|---|
| **Primary** | GSE73754 | Only dataset carrying disease status *and* HLA-B27 per sample, in one tissue, one platform, one experiment |
| **Level 2** | Geuvadis + 1000G HLA | Only source of healthy B27 carriers (33 vs 428) |
| **Gene set** | PXD055841 | 1,011 arthritogenic-peptide source proteins → custom set for `fgsea` |
| Optional | GSE25101, PXD024989 | External validation of the disease axis only |

---

## 1. Research question

**How much of the ankylosing-spondylitis expression signature in whole blood is attributable to HLA-B27 carriage, and how much to the disease itself?**

**Why this question and not another.** GSE73754 was generated to study *sexual dimorphism* in the Th17 signature (Gracey & Inman, [PMID 26473967](https://pubmed.ncbi.nlm.nih.gov/26473967/), *Arthritis Rheumatol* 2015). The HLA-B27 annotation sits in the sample records but is **not what the source paper analysed**. The B27 contrast in this dataset is therefore under-exploited.

---

## 2. What is actually in GSE73754

*Verified 2026-11-22 by parsing the series matrix, not from the GEO description.*

**Study:** "Sexual Dimorphism in the Th17 Signature of Ankylosing Spondylitis", Gracey E & Inman RD, University of Toronto. Public since 2015-10-07. [PMID 26473967](https://pubmed.ncbi.nlm.nih.gov/26473967/). BioProject PRJNA297828.
**Platform:** GPL10558, Illumina HumanHT-12 V4. **72 samples x 47,323 probes.** Whole blood, Tempus tubes, total RNA, biotin-labelled.

### Per-sample annotation

There are exactly **four** characteristics fields:

| Field | Values |
|---|---|
| `disease` | Ankylosing Spondylitis (52) / healthy control (20) |
| `hla-b27 (1=positive, 0=negative)` | 1 (39) / 0 (13) / unknown (20 — all the controls) |
| `Sex` | Male (37) / Female (35) |
| `age (yr)` | integer, 18-77, complete for all 72 |

Plus, recoverable from the sample titles (`AS [9963831033_D]`): the **Illumina BeadChip barcode and position**.

### Design Proposal

| Group | n | Female | Male | Age median (range) |
|---|---|---|---|---|
| AS, B27+ | 39 | 19 | 20 | 43 (18-77) |
| AS, B27− | 13 | 6 | 7 | 45 (18-60) |
| Healthy control | 20 | 10 | 10 | **34.5** (20-71) |

| BeadChip | AS B27+ | AS B27− | HC |
|---|---|---|---|
| 9963831032 | 8 | 0 | 4 |
| 9963831033 | 6 | 4 | 2 |
| 9963831045 | 7 | 2 | 3 |
| 9963831051 | 5 | 2 | 5 |
| 9965280034 | 5 | 4 | 3 |
| 9965280090 | 8 | 1 | 3 |

### Data files

- `GSE73754_non-normalized.txt.gz`: **raw Illumina intensities.** 
- `GSE73754_RAW.tar`: per-sample IDAT/raw files.

### What is *not* in it

No disease activity (BASDAI/ASDAS), no CRP or ESR, no symptom duration, no medication, no radiographic score, no B27 **subtype**, and **no B27 typing for the 20 healthy controls.** That last gap is the reason we need Geuvadis.
