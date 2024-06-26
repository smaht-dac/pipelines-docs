
# Long-Read PacBio HiFi

The long-read alignment pipeline for PacBio HiFi data is designed for per-sample and per-library execution, handling one or multiple unaligned BAM files. The pipeline is optimized for distributed processing, requiring each unaligned BAM file to correspond to a single SMRT Cell.

## Key Pipeline Steps

1. **Alignment with pbmm2:** Initial alignment of the raw reads to the reference genome using pbmm2.
2. **Read Groups Assignment:** Assignment of reads to specific groups.
3. **Methylation and Tags Linking:** Linking the methylation status information and other specific tags from the unaligned to the alignment BAM file.

## Pipeline Chart

![flow_chart](Flow_Chart_Pipeline.png)