# NGS Workflow: Commands and Explanations

This document records the main command-line operations used in the SARS-CoV-2 amplicon sequencing project.

## 1. Environment

The analysis was performed in Ubuntu using WSL2 on Windows. Bioinformatics tools were installed in a Conda environment named `ngs_env`.

Activate the environment:

```bash
conda activate ngs_env
```

## 2. Quality control

FastQC was used to assess individual FASTQ files.

```bash
fastqc ~/ngs_project/data/ERR5743893_1.fastq \
       ~/ngs_project/data/ERR5743893_2.fastq \
       -o ~/ngs_project/results/QC_Reports/
```

MultiQC was used to combine the quality-control reports.

```bash
multiqc ~/ngs_project/results/QC_Reports/ \
        -o ~/ngs_project/results/QC_Reports/
```

**Purpose:** FastQC identifies potential issues with sequencing reads, while MultiQC provides a combined summary.

## 3. Reference indexing

The SARS-CoV-2 reference genome was indexed with BWA.

```bash
bwa index ~/ngs_project/reference/NC_045512.2.fasta
```

**Purpose:** Creates index files that allow BWA to search the reference efficiently during alignment.

## 4. Read alignment

Paired-end reads were aligned to the reference genome using BWA-MEM.

```bash
bwa mem -t 4 \
  ~/ngs_project/reference/NC_045512.2.fasta \
  ~/ngs_project/data/ERR5743893_1.fastq \
  ~/ngs_project/data/ERR5743893_2.fastq \
  > ~/ngs_project/results/alignment/ERR5743893.sam
```

**Purpose:** Maps sequencing reads to the reference genome and records the alignments in SAM format. The `-t 4` option uses four threads.

## 5. Convert SAM to BAM

```bash
samtools view -b \
  ~/ngs_project/results/alignment/ERR5743893.sam \
  -o ~/ngs_project/results/alignment/ERR5743893.bam
```

**Purpose:** Converts the text-based SAM alignment into compressed binary BAM format.

## 6. Sort the BAM file

```bash
samtools sort \
  ~/ngs_project/results/alignment/ERR5743893.bam \
  -o ~/ngs_project/results/alignment/ERR5743893.sorted.bam
```

**Purpose:** Sorts alignments by reference position, which is required for common downstream operations.

## 7. Index the sorted BAM file

```bash
samtools index \
  ~/ngs_project/results/alignment/ERR5743893.sorted.bam
```

**Purpose:** Creates an index that enables efficient access to alignments at specific genomic positions.

## 8. Summarise alignment statistics

```bash
samtools flagstat \
  ~/ngs_project/results/alignment/ERR5743893.sorted.bam
```

**Purpose:** Reports the number of reads, mapped reads, properly paired reads and other alignment statistics.

Save a more detailed summary:

```bash
samtools stats \
  ~/ngs_project/results/alignment/ERR5743893.sorted.bam \
  > ~/ngs_project/results/alignment/ERR5743893.samtools_stats.txt
```

Display the summary statistics:

```bash
grep '^SN' \
  ~/ngs_project/results/alignment/ERR5743893.samtools_stats.txt
```

## 9. Genome coverage assessment

The next analysis step is to inspect coverage across the reference genome.

```bash
samtools coverage \
  ~/ngs_project/results/alignment/ERR5743893.sorted.bam
```

**Purpose:** Reports coverage metrics for the reference sequence. Coverage should be evaluated before variant calling, particularly for amplicon sequencing data.

## 10. Planned downstream analysis

After evaluating genome coverage, the workflow will proceed to variant calling and quality assessment using suitable tools such as FreeBayes or BCFtools.

These steps are planned and should not be considered completed until their commands have been run and their outputs reviewed.

## Notes

- Run commands in the Ubuntu terminal, not Windows PowerShell.
- File paths assume the project is located at `~/ngs_project`.
- Keep raw FASTQ files and large intermediate SAM/BAM files out of GitHub unless there is a specific reason to share them.
- Record tool versions and important analysis decisions to support reproducibility.
