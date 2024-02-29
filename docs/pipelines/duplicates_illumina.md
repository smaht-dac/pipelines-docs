---
layout: page
title: Duplicates
menubar: docs_menu
tabs: short_read_paired_end_illumina_tabs
show_sidebar: false
---

# Mark Duplicates

In this step, the pipeline marks duplicate reads, potential sequencing artifacts likely originating during library preparation or sequencing runs. Duplicate reads are evaluated per-library using the `LB` tag in the read groups. Instead of removing identified duplicate reads, the pipeline tags the reads directly in the BAM file. Additionally, an integrity check is performed on the output BAM file to confirm the presence of the 28-byte empty block representing the EOF marker in SAM format.

The implementation uses Sentieon LocusCollector to gather duplicate metrics per library and the Dedup algorithm to mark duplicate reads in the BAM file. Currently, Sentieon version 202308.01 is used, corresponding to Picard 2.9.0. The combined use of these two algorithms is equivalent to the MarkDuplicates algorithm in Picard.

All the relevant code can be accessed in the GitHub repository ([LocusCollector](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_LocusCollector.sh), [Dedup](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_LocusCollector_apply.sh)).

**Note:** It is possible to specify the `optical_dup_pix_dist` argument, representing the maximum offset between two duplicate clusters to consider them optical duplicates. For structured flow cells (NovaSeq, HiSeq 4000, X), the pipeline currently uses 2500.
