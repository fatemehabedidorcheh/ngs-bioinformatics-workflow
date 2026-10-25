## 8. Genome coverage results

Genome coverage was assessed using `samtools coverage` on the sorted BAM file.

| Metric | Result |
|---|---:|
| Reference genome | NC_045512.2 |
| Reference length | 29,903 bp |
| Positions covered by at least one read | 29,629 bp |
| Genome coverage | 99.08% |
| Mean depth | 2,195.74× |
| Mean base quality | Q35.5 |
| Mean mapping quality | 60 |

### Interpretation

The alignment covered 99.08% of the reference genome, indicating broad genome coverage. A total of 274 reference positions had no read coverage.

The mean depth was approximately 2,196×. However, average depth does not describe how evenly reads are distributed across the genome. Coverage gaps and low-depth regions should be examined before variant calling.

### Next step

Investigate the uncovered positions and assess the depth distribution across the reference genome. These checks will help determine whether the data are suitable for reliable downstream variant analysis.
