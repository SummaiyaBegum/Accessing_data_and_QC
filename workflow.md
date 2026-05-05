# 🧬 Detailed Workflow: NGS Quality Control Pipeline

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

## ⚙️ Tool Installation

```bash
sudo apt update
sudo apt install fastqc fastp multiqc -y
```

`sudo` runs commands with administrative privileges.
`apt update` refreshes the package list so the system knows the latest available software versions.
`apt install` installs FastQC, fastp, and MultiQC.
`-y` automatically confirms installation.

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
  Removes low-quality bases from the beginning of each read.

* `--cut_tail`
  Removes low-quality bases from the end of each read.
  Sequencing quality is usually lower at the ends, so trimming improves accuracy.

* `--cut_mean_quality 25`
  Trims regions where average quality drops below Q25.
  Q25 means high-confidence bases (~99.7% accuracy).

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
Reads below Q25 or shorter than 40 bases are discarded.
Summary reports are generated in HTML and JSON format.

---

## 🔍 QC After Trimming

```bash
fastqc ERR5386380_1.trimmed.fastq.gz ERR5386380_2.trimmed.fastq.gz
```

Confirms improvement in sequence quality after filtering.

---

## 📊 Summary Report

```bash
multiqc .
```

Aggregates all QC outputs into a single report for easy comparison of pre- and post-trimming results.

---

## 📈 Observations

* Read 2 showed lower quality compared to Read 1
* Quality improved after trimming
* Approximately 1% of reads were removed
* GC content remained consistent with expected organism profile

---

## 📁 Output Files

* FastQC reports (`*_fastqc.html`)
* fastp report (`fastp.html`)
* MultiQC report (`multiqc_report.html`)

---

## 📌 Conclusion

The dataset was successfully cleaned and validated.
The processed reads are suitable for downstream analysis such as alignment and variant calling.

---
