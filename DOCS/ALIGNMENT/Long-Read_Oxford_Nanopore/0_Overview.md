
# Long-Read Oxford Nanopore Technologies (ONT)

The long-read alignment pipeline for ONT data is designed for per-sample and per-library execution, handling one or multiple FASTQ files and the corresponding unaligned BAM files. The pipeline is optimized for distributed processing, requiring each FASTQ file to correspond to a single flow cell.

## Key Pipeline Steps

1. **Alignment with minimap2:** Initial alignment of the raw reads to the reference genome using minimap2.
2. **Read Groups Assignment:** Assignment of reads to specific groups.
3. **Methylation and Tags Linking:** Linking the methylation status information and other specific tags from the unaligned to the alignment BAM file.

## Pipeline Chart

![flow_chart](Flow_Chart_Pipeline.png)

---

<!-- This section relies on the html links generated by GitHub Pages 
and will not render correctly in Markdown -->
<div style="text-align: right">
    <a href="/pipelines-docs/"> Home </a> -
    <a> <b> Overview </b> </a> -
    <a href="1_Alignment.html"> Alignment </a> -
    <a href="2_Read_Groups.html"> Read Groups </a> -
    <a href="3_Methylation_and_Tags.html"> Methylation and Tags </a>
</div>