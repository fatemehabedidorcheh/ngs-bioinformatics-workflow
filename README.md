# NGS Quality Control Workflow

## Overview

This project covers the first steps of a next-generation sequencing (NGS) analysis. I use Linux on Windows (WSL2) to download public sequencing data, check its quality, and learn how to read the results before any downstream analysis.

Dataset: ERR5743893 (public data from the Sequence Read Archive, SRA).

Note: Before drawing any biological conclusion, you must know whether the sequencing data are reliable. Quality control (QC) answers that question.

## Objectives

- Download sequencing data from the SRA.
- Convert it to FASTQ format.
- Check read quality with FastQC.
- Interpret the QC results.
- Combine reports with MultiQC.
- Prepare for alignment and downstream analysis.

## Tools

| Tool | Purpose |
|---|---|
| Ubuntu on WSL2 | Linux command-line environment |
| SRA Toolkit | Download and convert sequencing data |
| FastQC | Check read quality |
| MultiQC | Combine QC reports (planned) |
| BWA | Align reads to a reference genome (planned) |
| SAMtools | Process alignments and statistics (planned) |
| BCFtools, FreeBayes | Possible variant calling (available) |

## Workflow

### 1. Download the data

```bash
cd ~/ngs_project/data
prefetch ERR5743893 --output-directory .
```

Learned: how to download public sequencing data and organise project folders.

Note: The SRA is a public archive of raw sequencing data. The file is downloaded in a compressed SRA format that most tools cannot read yet. ERR5743893 is the ID of this sequencing run.

### 2. Convert SRA to FASTQ

```bash
fasterq-dump ERR5743893/ERR5743893.sra \
  --split-files \
  --outdir .
```

Result: 198,733 spots read and 397,047 reads written.

Output files: ERR5743893_1.fastq, ERR5743893_2.fastq and ERR5743893.fastq. The first two are the paired-end reads (Read 1 and Read 2). The third file has not been checked yet.

Learned: how SRA data become FASTQ files and how command options change the output.

Note:
- FASTQ is the standard format for raw reads. Each read has its sequence and a quality score for every base.
- Paired-end sequencing reads each DNA fragment from both ends, giving one file per end.
- A spot is one sequenced cluster, which should give 2 reads. So 198,733 spots would give about 397,466 reads. The result (397,047) is slightly lower, so the third file may contain reads without a mate. This is a hypothesis I still need to check.

### 3. Run FastQC

```bash
mkdir -p ~/ngs_project/results/QC_Reports

fastqc \
  ~/ngs_project/data/ERR5743893_1.fastq \
  ~/ngs_project/data/ERR5743893_2.fastq \
  --outdir ~/ngs_project/results/QC_Reports
```

Output: one HTML report and one ZIP file per FASTQ file.

Learned: how to run a QC tool and find its results.

Note: FastQC does not change the data. It only measures quality, GC content, duplication and adapters, and labels each as PASS, WARN or FAIL. Read 1 and Read 2 are analysed separately and can differ.

### 4. First look at the QC results

I reviewed Read 1 (ERR5743893_1.fastq) first:

- Per-base quality: high, about Q34 to Q36 across most of the read.
- Per-base sequence composition: WARN.
- Per-sequence GC content: unusual distribution.
- Duplication and overrepresented sequences: high.
- Sequence length distribution: WARN (variable lengths).
- Adapter content: PASS.

A WARN or FAIL does not automatically mean contamination or bad data. The results must be read in light of the organism and library type.

Still to do: detailed review of Read 2 and the MultiQC report.

Learned: each QC metric measures a different property, so results need interpretation, not just PASS/WARN/FAIL labels.

Note:
- Q30 means about a 1 in 1,000 chance that a base is wrong, so Q34 to Q36 is very good.
- A composition warning can come from library preparation bias or low sequence diversity. It is not always a sequencing error.
- A clean single-species sample usually gives one bell-shaped GC peak. Odd or multiple peaks can mean mixed organisms or contamination, but can also be a natural feature of the genome or library.
- High duplication can come from PCR over-amplification, low-complexity input, or targeted sequencing (such as amplicons), where identical reads are expected. I cannot tell which applies until I identify the sample.
- Variable read lengths often mean the reads were trimmed before upload.
- Adapter PASS means adapter sequences are not heavily present.

## Status

Completed:
- Downloaded the data with SRA Toolkit.
- Converted it to FASTQ.
- Ran FastQC on both read files.
- Started interpreting Read 1.

Next steps:
- Review the Read 2 report.
- Generate and review a MultiQC report.
- Investigate GC content, duplication and overrepresented sequences.
- Decide whether trimming or filtering is needed.
- Identify the organism, reference genome and biological question.
- If appropriate, align the reads and check mapping statistics.

Note: The most important next step is finding out what the sample is. Without the organism and library type, I cannot tell whether the unusual QC results are real problems or normal features. I will check the SRA metadata.

## Data Management

Raw data and QC reports are stored locally and excluded from version control. This repository documents the commands, results and interpretation.

## Skills Practised

- Linux command line and file management.
- Retrieving public sequencing data.
- Working with SRA and FASTQ formats.
- Running bioinformatics tools.
- Assessing paired-end read quality.
- Interpreting QC metrics.
- Documenting a reproducible workflow.

This project is a work in progress and will be updated as the analysis develops.
