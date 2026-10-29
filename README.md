# SARS-CoV-2 NGS Bioinformatics Workflow

## Project Overview

This project documents a step-by-step bioinformatics workflow for analysing Illumina sequencing data from SARS-CoV-2. It covers data acquisition, sequencing quality control, reference genome preparation, read alignment, alignment statistics, and genome coverage assessment.

The project is designed to build practical skills in command-line bioinformatics, Linux, and reproducible genomic data analysis. Each stage includes the tools used, the purpose of the analysis, and the interpretation of the results.

**Current status:** Initial alignment and genome coverage assessment are complete. Investigation of uncovered regions and variant calling remain to be completed.

## Dataset and Reference Genome

- **Sample accession:** ERR5743893
- **Organism:** SARS-CoV-2
- **Sequencing platform:** Illumina NovaSeq 6000
- **Data type:** Paired-end sequencing reads
- **Reference genome:** SARS-CoV-2 Wuhan-Hu-1, NC_045512.2
- **Reference length:** 29,903 bp
- **Data source:** [NCBI Sequence Read Archive (SRA)](https://www.ncbi.nlm.nih.gov/sra)

The accession identifies the public sequencing dataset. The reference genome provides the coordinate system used to align reads and assess genomic coverage.

## Workflow and Progress

### 1. Data Acquisition

**Status: Completed**

Sequencing data were retrieved from the NCBI Sequence Read Archive and converted into FASTQ files using the SRA Toolkit.

FASTQ files contain nucleotide sequences and their corresponding per-base quality scores. Keeping paired-end reads correctly associated is important for subsequent alignment.

### 2. Sequencing Quality Control

**Status: Initial assessment completed**

FastQC was used to examine read quality, sequence composition, GC content, duplication levels, and overrepresented sequences. MultiQC was used to consolidate quality-control results into a summary report.

The reads showed generally high per-base quality scores. High duplication and overrepresented sequences were also observed. These findings require context: in amplicon sequencing, PCR amplification can produce substantial duplication, so these results should not automatically be interpreted as evidence of poor data quality.

See `docs/qc_interpretation.md` for the quality-control interpretation.

### 3. Reference Genome Preparation

**Status: Completed**

The SARS-CoV-2 reference genome NC_045512.2 was prepared for alignment, and the required BWA index files were generated.

Reference indexing allows the aligner to search the reference efficiently. The reference coordinates are also needed for interpreting alignment and coverage results.

### 4. Read Alignment and BAM Processing

**Status: Completed**

BWA-MEM was used to align sequencing reads to the reference genome. SAMtools was then used to process the alignment, sort the reads, index the sorted BAM file, and calculate alignment statistics.

SAM files contain alignment records in a text format. Sorted BAM files store the same type of information in a compressed binary format, while BAM index files support efficient access to genomic regions.

### 5. Initial Alignment Results

The initial alignment statistics were:

| Metric | Result |
|---|---:|
| Total alignment records, including supplementary records | 399,472 |
| Primary alignment records | 396,628 |
| Primary mapped records | 316,006 |
| Primary mapping percentage | 79.67% |
| Properly paired percentage | 77.93% |
| Average read length | 176 bp |
| Average base quality reported by SAMtools | Q35.5 |
| Mismatch rate reported by SAMtools | 0.52% |

These metrics describe the initial alignment. The primary mapping percentage excludes supplementary alignment records, so it differs slightly from the percentage calculated using all mapped records.

Mapping statistics should be interpreted alongside sequencing quality, library preparation, amplicon design, and coverage distribution. They do not independently establish the accuracy of variant calls.

### 6. Genome Coverage Assessment

**Status: Initial calculation completed; further investigation pending**

SAMtools coverage was used to assess how much of the reference genome had at least one aligned read.

| Metric | Result |
|---|---:|
| Reference genome length | 29,903 bp |
| Positions covered by at least one read | 29,629 bp |
| Reference genome covered | 99.08% |
| Mean sequencing depth | 2,195.74× |
| Positions with no read coverage | 274 |
| Mean base quality reported | Q35.5 |
| Mean mapping quality reported | 60 |

The reference was covered across approximately 99.08% of its length, with 274 positions having no observed read coverage.

Coverage percentage describes the proportion of reference positions covered by reads, whereas sequencing depth describes how many reads cover positions on average. A high mean depth does not guarantee uniform coverage across the genome.

**Next:** Investigate the distribution of coverage and the locations of uncovered positions before proceeding to variant calling.

See `docs/alignment_report.md` for the initial alignment and coverage results.

## Tools Used

- **SRA Toolkit:** Retrieval and conversion of public sequencing data.
- **FastQC:** Per-sample sequencing quality assessment.
- **MultiQC:** Consolidation of quality-control reports.
- **BWA-MEM:** Alignment of reads to the reference genome.
- **SAMtools:** Alignment processing, sorting, indexing, statistics, and coverage assessment.
- **Conda:** Management of the bioinformatics software environment.
- **Linux/WSL:** Command-line analysis environment.
- **Git and GitHub:** Version control and project documentation.

## Repository Structure

```text
ngs-bioinformatics-workflow/
├── README.md
├── .gitignore
├── commands/
│   ├── README.md
│   └── data_acquisition.md
└── docs/
    ├── alignment_report.md
    └── qc_interpretation.md
```

This structure separates the main project overview, command documentation, and analysis reports. Large raw sequencing files and intermediate alignment files are excluded from version control.

## Current Progress Checklist

- [x] Retrieve sequencing data
- [x] Generate FASTQ files
- [x] Run initial FastQC and MultiQC quality control
- [x] Prepare the reference genome for BWA
- [x] Align reads to the reference genome
- [x] Convert, sort, and index alignment files
- [x] Calculate alignment statistics
- [x] Calculate initial genome coverage
- [ ] Investigate coverage distribution and uncovered positions
- [ ] Perform and assess variant calling
- [ ] Document variant results and limitations

## Reproducibility and Data Management

Commands and analysis decisions are documented so that the workflow can be reviewed and repeated. Large raw sequencing files, reference index files, and intermediate alignment files are not intended to be committed to GitHub. Reports and documentation are retained to communicate the analysis and its limitations.

## Scope and Limitations

This repository documents a learning and analysis project using a public sequencing dataset. The reported mapping and coverage statistics are preliminary quality indicators. Further investigation is needed before drawing conclusions from genomic variants.

The workflow is still in progress; variant calling and interpretation have not yet been completed.
