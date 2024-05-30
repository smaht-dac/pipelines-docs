
# Long-Read Oxford Nanopore Technologies (ONT)

The long-read alignment pipeline for ONT data is designed for per-sample and per-library execution, handling one or multiple FASTQ files and the corresponding unaligned BAM files. The pipeline is optimized for distributed processing, requiring each FASTQ file to correspond to a single flow cell.

## Key Pipeline Steps

1. **Alignment with minimap2:** Initial alignment of the raw reads to the reference genome using minimap2.
2. **Read Groups Assignment:** Assignment of reads to specific groups.
3. **Methylation and Tags Linking:** Linking the methylation status information and other specific tags from the unaligned to the alignment BAM file.

## Pipeline Chart

![flow_chart](Flow_Chart_Pipeline.png)

---

<!-- This is html code that relies on the pages created by github to link PREV and NEXT page -->
<p style="text-align:left;">
    <a href="https://smaht-dac.github.io/pipelines-docs/"> PREV [<i> Home </i>] </a>
    <span style="float:right;">
        <a href="https://smaht-dac.github.io/pipelines-docs/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/1_Alignment.html"> NEXT [<i> Alignment </i>] </a>
    </span>
</p>
