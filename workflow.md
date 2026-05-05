# 🧬 Detailed Workflow: NGS Quality Control Pipeline

## 📌 Objective

To perform quality assessment and preprocessing of Illumina paired-end sequencing data using command-line bioinformatics tools in a reproducible manner.

---

## 🖥️ Environment Setup

### Install Required Tools

```bash
sudo apt update
sudo apt install fastqc fastp multiqc -y
```

**Explanation:**
Updates package lists and installs the required bioinformatics tools:

* FastQC → quality assessment
* fastp → trimming and filtering
* MultiQC → summarizing reports

---

### Verify Installation

```bash
fastqc --version
fastp --version
multiqc --version
```

**Explanation:**
Confirms that each tool is installed correctly and accessible in the system.

---

## 📁 Step 1: Create Working Directory

```bash
mkdir cp2
cd cp2
```

**Explanation:**
Creates a dedicated folder (`cp2`) to organize all files related to this analysis.

---

## ⬇️ Step 2: Download Raw Sequencing Data

```bash
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR538/000/ERR5386380/ERR5386380_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR538/000/ERR5386380/ERR5386380_2.fastq.gz
```

**Explanation:**
Downloads paired-end sequencing reads from the European Nucleotide Archive (ENA):

* `_1` → forward reads
* `_2` → reverse reads

---

## 🔍 Step 3: Initial Quality Assessment (FastQC)

```bash
fastqc ERR5386380_1.fastq.gz ERR5386380_2.fastq.gz
```

**Explanation:**
Evaluates raw sequencing quality:

* per-base quality scores
* GC content
* adapter contamination
* sequence duplication

---

## 📊 Step 4: View FastQC Reports

```bash
explorer.exe .
```

**Explanation:**
Opens the current directory in Windows Explorer (WSL environment) to view `.html` reports in a browser.

---

## ✂️ Step 5: Trimming and Filtering (fastp)

```bash
fastp \
--in1 ERR5386380_1.fastq.gz \
--in2 ERR5386380_2.fastq.gz \
--out1 ERR5386380_1.trimmed.fastq.gz \
--out2 ERR5386380_2.trimmed.fastq.gz \
--cut_front \
--cut_tail \
--cut_mean_quality 25 \
--length_required 40 \
--html fastp.html \
--json fastp.json
```

**Explanation:**
Performs read cleaning:

* removes low-quality bases from both ends
* filters reads below quality threshold (Q25)
* discards reads shorter than 40 bp
* generates QC reports (`fastp.html`, `fastp.json`)

---

## 🔍 Step 6: Quality Check After Trimming

```bash
fastqc ERR5386380_1.trimmed.fastq.gz ERR5386380_2.trimmed.fastq.gz
```

**Explanation:**
Validates improvement in sequencing quality after trimming.

---

## 📊 Step 7: Aggregate Reports (MultiQC)

```bash
multiqc .
```

**Explanation:**
Combines all QC outputs into a single report:

* compares before vs after trimming
* provides an overall quality summary

---

## 📈 Observations

* Read 2 showed lower quality compared to Read 1
* Quality improved significantly after trimming
* Approximately 1% of reads were removed
* GC content remained consistent with expected organism profile

---

## 📁 Output Summary

| File                  | Description                                 |
| --------------------- | ------------------------------------------- |
| `*_fastqc.html`       | Quality reports (before and after trimming) |
| `fastp.html`          | Detailed trimming report                    |
| `multiqc_report.html` | Combined QC summary                         |

---

## 📌 Conclusion

The sequencing data was successfully processed to remove low-quality bases and artifacts.
The cleaned reads are now suitable for downstream analysis such as:

* alignment
* variant calling
* antimicrobial resistance analysis

---
