
# Alignment with BWA-MEM

For the initial alignment, the pipeline uses BWA-MEM in paired-end mode on each set of paired FASTQ files. The reads are then sorted by genomic coordinates, and an integrity check is performed on the resulting BAM file.

## Aligning and Sorting

##### Align and sort reads

```text
sentieon bwa mem -K 10000000 reference.fasta reads.fastq mates.fastq |
  samtools sort -o sorted.bam -
```

Arguments:

- *-K*: chunk size option to have number of threads independent results.

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in BAM format.

## Implementation with Sentieon

Sentieon implementation replicates the original [BWA](https://github.com/lh3/bwa) code. The pipeline is using Sentieon version 202308.01, corresponding to BWA version 0.7.17.

*Note: In the original BWA code there is a programming error affecting alignment score calculations. Specifically, when secondary alignments are present, the score calculation includes unrelated information, potentially influencing the Mapping Quality (MAPQ) of the primary alignment by lowering it. Sentieon introduced a fix to the issue, resulting in slight differences in MAPQ between the two software (approximately 0.008% of reads are affected).*

## Source Code

All the relevant code can be accessed in the GitHub repository:

  - [sentieon_bwa-mem_sort.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_bwa-mem_sort.sh) [BWA-MEM]