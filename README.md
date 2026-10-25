# SARS-CoV-2 NGS Bioinformatics Workflow

A step-by-step bioinformatics project for processing and analysing paired-end Illumina sequencing data from SARS-CoV-2 using Linux command-line tools.

The project demonstrates a reproducible workflow from public sequencing data acquisition and quality control to reference alignment and downstream variant analysis.

## Project objectives

- Retrieve publicly available sequencing data.
- Assess read quality using FastQC and MultiQC.
- Document sequencing metadata and quality-control findings.
- Align reads to the SARS-CoV-2 reference genome.
- Process and evaluate alignment files using SAMtools.
- Assess genome coverage before proceeding to variant calling.
- Identify and interpret sequence variants in subsequent workflow stages.

## Dataset

| Field | Information |
|---|---|
| Accession | ERR5743893 |
| Organism | SARS-CoV-2 |
| Sample source | Viral RNA |
| Library strategy | Amplicon sequencing |
| Library selection | PCR |
| Sequencing platform | Illumina NovaSeq 6000 |
| Read layout | Paired-end |
| Reference genome | NC_045512.2, SARS-CoV-2 Wuhan-Hu-1 |

The dataset metadata are stored in `reports/ERR5743893_metadata.tsv`.

## Workflow

### 1. Environment and tool setup

The workflow is being developed in Ubuntu using WSL2 on Windows, with a Conda environment for bioinformatics tools.

Tools used include:

- SRA Toolkit for data retrieval and FASTQ conversion
- FastQC for read-quality assessment
- MultiQC for summarising quality-control reports
- BWA for sequence alignment
- SAMtools for alignment processing and statistics
- BCFtools and FreeBayes for planned downstream variant analysis

### 2. Data acquisition and quality control

The sequencing data were retrieved from the public SRA archive and converted into FASTQ files.

FastQC was used to assess read quality, read length, GC content, duplication and adapter content. MultiQC was used to combine the quality-control results into a single report.

Reports are stored in `results/QC_Reports/`.

### 3. Reference preparation

The SARS-CoV-2 reference genome, `NC_045512.2.fasta`, was downloaded and indexed for alignment.

Reference files are stored in `reference/`.

### 4. Read alignment

Paired-end reads were aligned to the reference genome using BWA-MEM.

The SAM alignment was converted to BAM, sorted and indexed using SAMtools.

Main alignment files are stored in `results/alignment/`.

### 5. Preliminary alignment statistics

Initial SAMtools statistics showed:

| Metric | Result |
|---|---:|
| Primary alignment records | 396,628 |
| Primary mapped records | 316,006 |
| Primary mapping percentage | 79.67% |
| Properly paired reads | 77.93% |
| Average read length | 176 bp |
| Average base quality | Q35.5 |
| Reported mismatch rate | 0.52% |

These are preliminary alignment metrics. Mapping percentage and mismatch rate must be interpreted in the context of the amplicon library, reference genome and coverage distribution.

### 6. Genome coverage and variant analysis

**Status: In progress**

The next steps are to evaluate reference-genome coverage, investigate uncovered regions and assess whether the alignment is suitable for downstream variant calling. Variant identification and interpretation will be documented after these analyses are completed.

## Repository structure

```text
ngs-bioinformatics-workflow/
├── README.md
├── .gitignore
├── data/
├── reference/
├── results/
│   ├── QC_Reports/
│   └── alignment/
├── reports/
├── commands/
└── docs/
```

Large sequencing files and generated alignment files are kept out of version control where appropriate. The repository focuses on documentation, commands, analysis summaries and reproducible workflow development.

## Reproducibility

Commands and analysis decisions are documented as the workflow progresses. Tool versions, dataset metadata, reference accession and quality-control findings are recorded to support reproducibility.

## Limitations

This is an educational bioinformatics project using a public sequencing dataset. Alignment statistics alone do not establish variant accuracy. Coverage assessment, variant quality evaluation and appropriate biological interpretation are required before drawing conclusions.

## Project status

- [x] Data acquisition and FASTQ conversion
- [x] FastQC and MultiQC quality assessment
- [x] Dataset metadata documentation
- [x] Reference genome preparation
- [x] BWA-MEM read alignment
- [x] BAM conversion, sorting and indexing
- [x] Initial alignment statistics
- [ ] Genome coverage assessment
- [ ] Variant calling and quality assessment
- [ ] Variant interpretation and final report
