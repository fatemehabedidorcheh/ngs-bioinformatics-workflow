# NGS Bioinformatics Workflow

## Project Overview

This is a hands-on bioinformatics project focused on learning and implementing a next-generation sequencing (NGS) workflow using public sequencing data. The project uses Linux through Windows Subsystem for Linux (WSL2) and command-line bioinformatics tools.

The dataset used is **ERR5743893**, a paired-end SARS-CoV-2 amplicon sequencing dataset from the Illumina NovaSeq 6000 platform.

## Project Objectives

- Retrieve public sequencing data from the Sequence Read Archive (SRA).
- Convert SRA data into FASTQ files.
- Assess sequencing quality using FastQC.
- Combine and interpret quality-control results using MultiQC.
- Retrieve and prepare a reference genome for read alignment.
- Progress toward alignment, mapping-quality assessment, and downstream sequence analysis.

## Tools and Technologies

- **Operating system:** Ubuntu Linux through WSL2
- **SRA Toolkit:** `prefetch`, `fasterq-dump`
- **Quality control:** FastQC, MultiQC
- **Read alignment:** BWA
- **Reference genome:** NCBI accession NC_045512.2
- **Additional tools available in the environment:** SAMtools, BCFtools, FreeBayes

## Workflow and Progress

### 1. Data Acquisition

Retrieved public sequencing data with SRA accession `ERR5743893` using the SRA Toolkit.

### 2. SRA to FASTQ Conversion

Converted the SRA file into FASTQ format using `fasterq-dump` with the `--split-files` option. Paired-end reads were saved as separate Read 1 and Read 2 files.

### 3. Quality Control with FastQC

Ran FastQC on both FASTQ files and generated individual HTML reports and ZIP files.

Initial findings included:

- High per-base sequence quality in both read files.
- Warnings or failures for sequence composition, GC distribution, duplication, and overrepresented sequences.
- Adapter-content checks passed for the adapter categories assessed.

These findings require interpretation in the context of the confirmed amplicon library type.

### 4. Combined Quality Control with MultiQC

Generated a combined report named `multiqc_report.html` using MultiQC. The report includes the results from both FastQC files.

The report was generated successfully and is stored locally in `results/QC_Reports/`.

### 5. Dataset Metadata

Retrieved run metadata from the European Nucleotide Archive (ENA).

- **Run accession:** ERR5743893
- **Organism:** Severe acute respiratory syndrome coronavirus 2 (SARS-CoV-2)
- **Library strategy:** AMPLICON
- **Library source:** VIRAL RNA
- **Library selection:** PCR
- **Library layout:** PAIRED
- **Sequencing platform:** Illumina
- **Instrument:** Illumina NovaSeq 6000

The metadata was saved locally as `reports/ERR5743893_metadata.tsv`.

### 6. Reference Genome Preparation

Downloaded the SARS-CoV-2 Wuhan-Hu-1 reference genome, accession `NC_045512.2`, in FASTA format.

The reference file is stored locally as `reference/NC_045512.2.fasta`.

Installed BWA version 0.7.17 to prepare for read alignment. Reference indexing and alignment are the next planned steps and have not yet been confirmed as completed.

## Data Management

Raw sequencing files and generated quality-control reports are retained locally and excluded from version control using `.gitignore`. This avoids uploading large data files and generated reports to the repository.

## Skills Practiced

- Linux command-line operations
- Working with public NGS datasets
- SRA data retrieval and FASTQ conversion
- Sequencing quality assessment and report interpretation
- Metadata retrieval and interpretation
- Reference genome preparation
- Use of bioinformatics command-line tools
- Reproducible workflow documentation

## Project Status

**Completed:** SRA retrieval, FASTQ conversion, FastQC analysis of both reads, MultiQC report generation, dataset metadata retrieval, reference genome download, and BWA installation.

**In progress:** Reference indexing and preparation for read alignment.

**Planned:** Read alignment, mapping-quality assessment, and appropriate downstream sequence analysis.

This project is ongoing, and the repository will be updated as additional workflow stages are completed.
