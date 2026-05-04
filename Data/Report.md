# 📊 Report: Quality Control and Trimming of WGS Reads

## 1️⃣ Initial Quality Assessment (Raw Reads)

FastQC analysis of raw paired-end reads showed overall high sequencing quality. Read 1 exhibited consistently high per-base quality across the read length, whereas read 2 showed a noticeable decline in quality toward the 3′ end, a common characteristic of Illumina paired-end sequencing.

The observed GC content (~66%) was consistent with the known high-GC genome of *Pseudomonas aeruginosa*, indicating no evidence of contamination or sample misidentification. These findings justified quality-based trimming prior to downstream analysis.

---

## 2️⃣ Read Trimming and Filtering

Reads were trimmed using fastp to remove low-quality bases, adapter contamination, and excessively short reads. Trimming was applied uniformly to both paired-end reads to preserve pairing integrity.

Filtering parameters were selected to balance read quality improvement with maximal data retention.

---

## 3️⃣ fastp Summary Statistics

### Read Quality Improvement
- **Read 1 Q30**
  - Before filtering: 94.49%
  - After filtering: 94.94%
- **Read 2 Q30**
  - Before filtering: 89.87%
  - After filtering: 91.31%

These results demonstrate a clear improvement in base-level accuracy, particularly for read 2.

---

### Read Retention
- Approximately 1.2% of total reads were removed
- Majority of removed reads were due to low base quality
- Very few reads were discarded due to ambiguous bases (Ns) or insufficient length after trimming

This indicates that trimming was effective without excessive loss of sequencing data.

---

### Adapter and Duplication Metrics
- Adapter contamination was minimal and successfully removed
- Duplication rate (~13%) was consistent with high-coverage bacterial WGS data
- Insert size peak (~270 bp) matched expectations for Illumina libraries

---

## 4️⃣ Post-Trimming Quality Validation

FastQC was re-run on the trimmed reads. Compared to the raw data, the post-filtering reports showed:
- reduced low-quality tails, particularly in read 2
- improved per-base quality distributions
- preserved GC content and read length profiles

No new biases were introduced during trimming.

---

## ✅ Final Assessment

Quality filtering substantially improved sequencing accuracy while preserving nearly all sequencing data. The processed paired-end reads are of high quality and suitable for downstream genomic analyses such as genome assembly and antimicrobial resistance gene detection.

---

## 🧠 Key Takeaway

This workflow demonstrates an evidence-driven quality control strategy in which diagnostic results guided trimming decisions, and improvements were quantitatively and visually validated.
