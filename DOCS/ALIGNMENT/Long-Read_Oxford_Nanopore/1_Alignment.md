
# Alignment with minimap2

For alignment, the pipeline uses minimap2 on each FASTQ file. The reads are then sorted by genomic coordinates, and an integrity check is performed on the resulting BAM file.

## Aligning and Sorting

##### Align and sort reads

```text
sentieon minimap2 -Y -L --eqx --secondary=no -ax map-ont reference.fasta reads.fastq |
  samtools sort -o sorted.bam -
```

Arguments:

- *-Y*: enable soft clipping.
- *-L*: use long cigars for tag `CG`.
- *-\-eqx*: use X/= in cigars instead of M.
- *-\-secondary=no*: ignore secondary alignments.

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in BAM format.

## Implementation with Sentieon

Sentieon implementation replicates the original [minimap2](https://github.com/lh3/minimap2) code. The pipeline is using Sentieon version 202308.01, corresponding to minimap2 version 2.26.

## Source Code

All the relevant code can be accessed in the GitHub repository:

  - [sentieon_minimap2_sort.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_minimap2_sort.sh) [minimap2]

---

<!-- This is html code that relies on the pages created by github to link PREV and NEXT page -->
<p style="text-align:left;">
    <a href="https://smaht-dac.github.io/pipelines-docs/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/0_Overview.html"> PREV [<i> Overview </i>] </a>
    <span style="float:right;">
        <a href="https://smaht-dac.github.io/pipelines-docs/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/2_Read_Groups.html"> NEXT [<i> Read Groups </i>] </a>
    </span>
</p>
