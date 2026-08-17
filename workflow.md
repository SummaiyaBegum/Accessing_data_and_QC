# 🧬 Accessing Data and Quality Control

## 📌 Objective

Perform quality assessment and preprocessing of Illumina paired-end sequencing data in a reproducible Linux (Ubuntu) environment.

---

## 🖥️ Working Environment

All commands are executed in **Ubuntu (WSL)** because bioinformatics tools are designed for Linux systems and work reliably with large sequencing datasets.

Basic commands used:

* `pwd` → shows current directory
* `ls` → lists files
* `mkdir` → creates folders
* `cd` → moves between folders
* `wget` → downloads data

---

## 📁 Create Working Directory

```bash
mkdir qc
cd qc
pwd
ls
```

A dedicated folder is created and entered to keep all analysis files organized.
`pwd` confirms the current location and `ls` verifies contents.
---

## ⚙️ Tool Installation

```bash
sudo apt update
sudo apt install fastqc fastp multiqc -y
```

`sudo` runs commands with administrative privileges.
`apt update` refreshes the local package index so Ubuntu knows which packages and versions are available from its configured repositories.
`apt install` installs FastQC, fastp, and MultiQC.
`-y` automatically confirms installation.
---

## ⬇️ Download Raw Data

```bash
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR538/000/ERR5386380/ERR5386380_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR538/000/ERR5386380/ERR5386380_2.fastq.gz
ls
```

Paired-end sequencing reads are downloaded from ENA.
`_1` represents forward reads and `_2` represents reverse reads.
`ls` confirms successful download.

---

## 🔍 Initial Quality Check

```bash
fastqc ERR5386380_1.fastq.gz ERR5386380_2.fastq.gz
```

FastQC analyzes raw reads for base quality, GC content, duplication levels, and potential adapter contamination.

---

## 📊 View Reports

```bash
explorer.exe .
```

Opens the current directory to access HTML reports in a browser.

---

## ✂️ Trimming and Filtering

```bash
fastp --in1 ERR5386380_1.fastq.gz --in2 ERR5386380_2.fastq.gz --out1 ERR5386380_1.trimmed.fastq.gz --out2 ERR5386380_2.trimmed.fastq.gz --cut_front --cut_tail --cut_mean_quality 25 --length_required 40 --html fastp.html --json fastp.json
```
## 🔧 fastp Command Breakdown
* `fastp`
  Runs the trimming and quality filtering tool.

* `--in1 ERR5386380_1.fastq.gz`
  Input file for forward reads (Read 1).

* `--in2 ERR5386380_2.fastq.gz`
  Input file for reverse reads (Read 2).
  These two files are paired-end reads from the same DNA fragments.

* `--out1 ERR5386380_1.trimmed.fastq.gz`
  Output file for cleaned forward reads.

* `--out2 ERR5386380_2.trimmed.fastq.gz`
  Output file for cleaned reverse reads.
  These trimmed files will be used for alignment.

* `--cut_front`
  Enables quality-based cutting from the beginning of the read.

* `--cut_tail`
  Enables quality-based cutting from the end of the read.

* `--cut_mean_quality 25`
  Sets the mean quality threshold used during quality-based cutting.
  In this workflow, fastp uses Q25 as the threshold when deciding where to cut low-quality regions.
  Q25 corresponds to approximately 99.68% expected base-call accuracy.

* `--length_required 40`
  Removes reads shorter than 40 bases after trimming.
  Very short reads are unreliable and may map incorrectly.

* `--html fastp.html`
  Creates a visual report showing quality before and after trimming.

* `--json fastp.json`
  Creates a structured report for further analysis or pipeline use.

---

### 🧠 Summary

This step cleans the raw sequencing data by removing low-quality bases and unreliable reads, making the data ready for accurate alignment and downstream analysis.

Low-quality bases are removed from both ends of reads.
The quality-based cutting settings remove low-quality regions, while `--length_required 40` filters out reads that are shorter than 40 bases after processing.
Summary reports are generated in HTML and JSON format.

---

## 🔍 QC After Trimming

```bash
fastqc ERR5386380_1.trimmed.fastq.gz ERR5386380_2.trimmed.fastq.gz
```

The trimmed reads were assessed again with FastQC so that the results could be compared with the initial QC.

---

## 📊 Summary Report

```bash
multiqc .
```

Aggregates all QC outputs into a single report for easy comparison of pre- and post-trimming results.

---

## 📈 Observations

- Read 2 showed lower quality compared to Read 1.
- Quality improved after trimming.
- Approximately 1% of reads were removed during processing.
- GC content remained relatively consistent before and after processing.
---

## 📁 Output Files

* FastQC reports (`*_fastqc.html`)
* fastp report (`fastp.html`)
* MultiQC report (`multiqc_report.html`)

---

## 📌 Conclusion

The raw Illumina reads were assessed using FastQC, processed using fastp, and assessed again after trimming. MultiQC was then used to summarize the QC results.

Based on the QC results from this dataset, the processed reads can be taken forward to the next stage of the workflow, where I will explore alignment and variant calling.
---
