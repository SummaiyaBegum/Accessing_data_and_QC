# 🧬 Illumina NGS Quality Control Workflow

## 📌 Overview
This project demonstrates a reproducible workflow for quality control and preprocessing of Illumina paired-end sequencing data using FastQC, fastp, and MultiQC.

The goal is to assess sequencing quality, identify issues, perform trimming, and validate improvements before downstream analysis.

---

## 🧪 Dataset
- Organism: *Pseudomonas aeruginosa*
- Source: European Nucleotide Archive (ENA)
- Accession: ERR5386380
- Sequencing: Illumina paired-end

---

## 🔄 Workflow

1. Download raw FASTQ files from ENA  
2. Perform initial QC using FastQC  
3. Trim low-quality reads using fastp  
4. Re-run FastQC on trimmed reads  
5. Summarize results using MultiQC  

---

## 📊 Key Observations

- Read 2 showed lower quality compared to Read 1
- Trimming improved overall base quality
- Minimal reads removed (~1%)
- GC content consistent with *P. aeruginosa*

---

## 🧰 Tools Used

- FastQC v0.11.9
- fastp v0.20.1
- MultiQC v1.x

---

## 📁 Repository Structure
qc/ → FastQC reports
reports/ → fastp and MultiQC outputs


---

## 🚀 Next Steps

- Alignment to reference genome (BWA)
- Variant calling (bcftools)
- AMR analysis

---

## 📌 Note

Raw FASTQ files are not included due to size constraints.

