---
layout: page
menubar: docs_menu
tabs: short_read_paired_end_illumina_tabs
show_sidebar: false
hide_hero: true
---

# Hi-C

For the alignment of Hi-C data, the pipeline uses BWA-MEM in paired-end mode on each set of paired FASTQ files. The reads are then sorted by genomic coordinates, and an integrity check is performed on the resulting BAM file. Compared to standard alignment, extra flags are required to support the read pairs generated for the Hi-C data.

## Aligning and Sorting

##### Align and sort reads

```text
sentieon bwa mem -5SP -K 10000000 reference.fasta reads.fastq mates.fastq |
  samtools sort -o sorted.bam -
```

Standard arguments:

- *-K*: chunk size option to have number of threads independent results.

Hi-C specific arguments:

- *-S* skip mate rescue.
- *-P* skip pairing. Mate rescue performed unless -S also in use.
- *-5* for split alignment, take the alignment with the smallest coordinate as primary.

*-SP* ensures that the results are equivalent to aligning each mate separately, while maintaining the proper paired-end read formatting. It also avoids forced alignment of a poorly aligned read given an alignment of its mate, based on assumption that the two mates belong to a single genomic segment.

*-5* ensures that the 5' portion of chimeric alignments is marked as the primary alignment. In Hi-C experiments, the 5' portion of the read is typically the alignment of interest, with the 3' portion representing the same fragment as the mate.

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in SAM format.

## Implementation with Sentieon

Sentieon implementation replicates the original [BWA](https://github.com/lh3/bwa) code. The pipeline is using Sentieon version 202308.01, corresponding to BWA version 0.7.17.

## Source Code

All the relevant code can be accessed in the GitHub repository:

  - [sentieon_bwa-mem_sort_Hi-C.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_bwa-mem_sort_Hi-C.sh) [BWA-MEM]
