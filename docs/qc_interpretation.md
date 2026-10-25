# FastQC Interpretation Notes

## Dataset

- Accession: ERR5743893
- Read reviewed in detail: Read 1 (ERR5743893_1.fastq)
- QC tool: FastQC 0.12.1

## Initial Results

| Metric | Result | Comment |
|---|---|---|
| Per-base sequence quality | PASS | Average quality about Q34 to Q36 across much of the read |
| Per-base sequence content | WARN | Base composition is uneven along the read |
| Per-sequence GC content | FAIL | GC distribution is unusual |
| Sequence length distribution | WARN | Reads have variable lengths |
| Sequence duplication levels | FAIL | Deduplicated percentage about 4.73% |
| Overrepresented sequences | FAIL | Some sequences occur far more often than expected |
| Adapter content | PASS | No adapters detected in the categories FastQC checks |

Note: Q30 means about a 1 in 1,000 chance that a base is wrong, so Q34 to Q36 is very good. A deduplicated percentage of 4.73% means only about 5% of the reads are unique sequences, so most reads are copies of others.

## What Each Result May Mean

Note:
- **Per-base sequence content (WARN):** In a random library, A, C, G and T should be roughly balanced along the read. Uneven composition can come from library preparation bias, low sequence diversity, or a targeted sample. It is not always a sequencing error.
- **Per-sequence GC content (FAIL):** A clean single-species genome usually gives one smooth, bell-shaped peak. An odd or multi-peaked curve can mean mixed organisms or contamination, but it can also be a natural feature of the genome or library type. FastQC compares against a theoretical curve, so it can fail even for good data.
- **Sequence length distribution (WARN):** Variable lengths often mean the reads were trimmed before upload. This is not necessarily a problem.
- **Sequence duplication (FAIL) and overrepresented sequences (FAIL):** These can come from PCR over-amplification, very low-complexity input, or targeted sequencing such as amplicons, where identical reads are expected. They are also seen with very small genomes sequenced at high depth. Which cause applies depends on the sample, which I have not yet identified.
- **Adapter content (PASS):** A good sign that adapter sequences are not heavily present in the reads.

## Interpretation

High base-call quality does not guarantee that every other QC metric will pass. GC content, duplication and overrepresented sequences can be affected by the biology of the sample, library preparation, amplification, or technical factors.

These observations alone do not show contamination and do not prove that the data are unsuitable for analysis. The sample type, organism and library preparation must be considered before deciding whether preprocessing is needed.

Note: PASS, WARN and FAIL are guidelines, not verdicts. FastQC was designed with standard whole-genome libraries in mind, so other library types (such as amplicon or viral sequencing) often trigger warnings that are normal for them.

## Remaining Work

- Review Read 2 in detail.
- Generate and inspect the MultiQC summary.
- Investigate the unusual QC metrics in the context of the dataset, starting with the SRA metadata (organism and library strategy).
- Document any preprocessing decision and compare QC results before and after preprocessing.
