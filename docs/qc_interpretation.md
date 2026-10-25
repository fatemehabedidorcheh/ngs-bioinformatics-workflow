# Quality Control Report: SARS-CoV-2 Amplicon Sequencing

## Dataset

- **SRA accession:** ERR5743893
- **Organism:** SARS-CoV-2
- **Library strategy:** Amplicon sequencing
- **Sequencing platform:** Illumina NovaSeq 6000
- **Read layout:** Paired-end

## Tools

- **FastQC:** Assesses the quality and characteristics of sequencing reads.
- **MultiQC:** Combines quality-control reports into a single summary report.

## Observed results

The paired-end FASTQ files each contained 198,314 reads.

| Metric | Read 1 | Read 2 |
|---|---:|---:|
| Average sequence quality | Approximately Q35–36 | Approximately Q33–36 |
| GC content | Approximately 39% | Approximately 39% |
| Sequence length range | 20–221 bp | 20–201 bp |
| Duplicate sequences | 95.3% | 94.1% |
| Overrepresented sequences | 87.6% | 85.8% |

These values were recorded from the FastQC/MultiQC results and should be interpreted in the context of the library preparation method.

## Interpretation

### Read quality

The reported average base qualities were high. However, average quality alone does not establish that every base or read is reliable.

### Read length

The reads had variable lengths, ranging from 20 bp to 221 bp for Read 1 and 20 bp to 201 bp for Read 2.

### GC content and sequence composition

The reported GC content was approximately 39%. Sequence-composition warnings should be considered alongside the expected characteristics of the viral amplicon library.

### Duplication and overrepresented sequences

FastQC reported high duplication and a high proportion of overrepresented sequences. These findings require careful interpretation because amplicon sequencing uses PCR to enrich specific target regions, which can naturally produce repeated sequences.

High duplication alone does not prove contamination, and reads should not be removed solely because they are duplicates.

### Adapter content

The adapter-content module was reported as passing. This indicates that adapter contamination was not flagged as a major issue by this FastQC module.

## Conclusion

The initial quality-control results indicate high average base quality, alongside substantial sequence duplication and overrepresentation. These findings must be evaluated in the context of the amplicon-based library.

Quality control was followed by alignment to the SARS-CoV-2 reference genome NC_045512.2. Mapping statistics and genome coverage are needed to further assess the data before variant calling.

## Output files

- `results/QC_Reports/ERR5743893_1_fastqc.html`
- `results/QC_Reports/ERR5743893_2_fastqc.html`
- `results/QC_Reports/multiqc_report.html`
