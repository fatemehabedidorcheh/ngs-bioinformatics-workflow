# Alignment Report: SARS-CoV-2 Amplicon Sequencing

## 1. Objective

The objective of this stage was to align paired-end Illumina sequencing reads from the SARS-CoV-2 dataset ERR5743893 to the reference genome NC_045512.2 and evaluate the resulting alignments.

## 2. Tools

- **BWA-MEM 0.7.17:** Aligns sequencing reads to a reference genome.
- **SAMtools:** Converts, sorts, indexes and evaluates alignment files.

## 3. Alignment workflow

The paired-end FASTQ files were aligned using BWA-MEM. The resulting SAM file was converted to BAM format, sorted by genomic position and indexed using SAMtools.

The main output files were:

- `ERR5743893.sam`
- `ERR5743893.bam`
- `ERR5743893.sorted.bam`
- `ERR5743893.sorted.bam.bai`

The sorted BAM file was used to calculate alignment statistics.

## 4. Alignment results

| Metric | Result |
|---|---:|
| Primary alignment records | 396,628 |
| Primary mapped records | 316,006 |
| Primary mapping percentage | 79.67% |
| Properly paired reads | 77.93% |
| Supplementary alignments | 2,844 |
| Singletons | 30 |
| Average read length | 176 bp |
| Average base quality | Q35.5 |
| Reported mismatch rate | 0.52% |
| Average insert size | 385 bp |

These results were obtained from the SAMtools `flagstat` and `stats` outputs.

## 5. Interpretation

### Mapping percentage

Approximately 79.67% of primary alignment records mapped to the reference genome. This indicates that most primary reads aligned, but the unmapped fraction requires further investigation.

### Properly paired reads

Approximately 77.93% of reads were reported as properly paired. This metric reflects whether read pairs satisfy the aligner's criteria for expected pairing and orientation.

### Supplementary alignments

The 2,844 supplementary alignment records represent additional alignment segments. They should not automatically be interpreted as separate sequencing reads or as evidence of contamination.

### Mismatch rate

SAMtools reported 341,280 mismatches and an error rate of approximately 0.52%. This rate describes mismatches in the aligned data; it is not a direct measurement of the sequencing instrument's error rate. Mismatches may reflect sequencing errors, genuine sequence differences, or alignment-related effects.

### Insert size

The reported average insert size was 385 bp, with a standard deviation of 41.7 bp. This estimate should be interpreted in the context of the library preparation and alignment characteristics.

## 6. Limitations and next steps

Alignment statistics alone are insufficient to establish the quality of the dataset or the reliability of future variant calls.

The next steps are to:

1. Assess coverage across the SARS-CoV-2 reference genome.
2. Identify regions with low or absent coverage.
3. Investigate the unmapped reads if necessary.
4. Evaluate whether the alignment is suitable for variant calling.
5. Perform variant calling and assess the quality of candidate variants.

## 7. Conclusion

The paired-end reads were successfully aligned to the SARS-CoV-2 reference genome, and the resulting BAM file was sorted and indexed. Initial statistics indicate that most primary reads mapped, while further coverage assessment is needed before proceeding to variant calling.

**Status:** Alignment completed; coverage assessment pending.
