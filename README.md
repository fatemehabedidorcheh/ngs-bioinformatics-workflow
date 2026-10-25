# NGS Bioinformatics Workflow

## About this project

This is my learning project for next-generation sequencing (NGS) data analysis. I use Linux on Windows (WSL2) and public sequencing data.

The dataset is ERR5743893. It is SARS-CoV-2 (the COVID-19 virus) sequencing data from an Illumina NovaSeq 6000 machine. The virus genome was amplified by PCR before sequencing (amplicon sequencing).

## Tools

- SRA Toolkit: download data
- FastQC and MultiQC: check data quality
- BWA: map reads to the reference genome
- SAMtools: process and check the mapping
- Reference genome: NC_045512.2 (SARS-CoV-2)

## What I have done

### 1. Downloaded the data

I downloaded the dataset from the Sequence Read Archive (SRA).

### 2. Converted it to FASTQ

I converted the SRA file into FASTQ files. The data are paired-end, so there is a Read 1 file and a Read 2 file. Each contains 198,314 reads.

### 3. Checked the quality

I ran FastQC on both files and combined the results with MultiQC.

- The sequencing quality is very high.
- No adapter sequences were found.
- Duplication is very high (about 95%), and the GC content and base composition show warnings.

These warnings are normal for amplicon data, because PCR makes many copies of the same pieces. They do not mean the run failed. For this reason I will not remove duplicates.

### 4. Checked the sample information

From the ENA database: organism SARS-CoV-2, library type amplicon, source viral RNA, paired-end reads.

### 5. Got the reference genome

I downloaded the SARS-CoV-2 reference genome (NC_045512.2) and installed BWA.

## Next step: mapping

Mapping finds where each read fits on the reference genome. The plan is:

1. Index the reference genome.
2. Map the reads with BWA.
3. Convert the result to a sorted BAM file.
4. Check how many reads mapped and how well the genome is covered.

## Notes on data

Large data files are kept on my computer only and are not uploaded to GitHub.

## Status

Done: download, FASTQ conversion, quality control, sample information, reference genome.

Next: indexing the reference and mapping the reads.

I will update this page as I complete each step.
