---
layout: page
title: polyG Artifacts
menubar: docs_menu
tabs: preprocessing_fastq_tabs
show_sidebar: false
---

# polyG Artifacts Removal

Latest Illumina technologies using two-channel sequencing systems, such as NextSeq and NovaSeq, may introduce homopolymer runs of Gs (polyG) as artifacts. Poly-G artifacts appear when the dark base G is called after the synthesis has terminated, resulting in the erroneous calling of high-confidence G bases at the ends of affected reads. Eventually, a large number of these reads may align to reference regions with high G content (e.g., chr2:32916230-32916625), creating problems for downstream processing.

As part of FASTQ files preprocessing, raw reads generate by Illumina two-channel sequencing systems are filtered using fastp to remove read pairs containing polyG artifacts.

## Removing Read Pairs Containing polyG Artifacts

##### Filter read pairs trimmed for polyG artifacts

```text
fastp
    --dont_eval_duplication
    --disable_adapter_trimming
    --disable_quality_filtering
    --trim_poly_g
    --length_required read_length
    -i reads.fastq -I mates.fastq
    -o reads.filtered.fastq -O mates.filtered.fastq
```

Arguments:

- *-\-dont_eval_duplication*: disable duplication evaluation.
- *-\-disable_adapter_trimming*: disable adapter trimming.
- *-\-disable_quality_filtering*: disable quality filtering.
- *-\-trim_poly_g*: activate polyG tail trimming.
- *-\-length_required*: discard reads shorter than the specified read length.

## Implementation with fastp

The pipeline is using [fastp](https://github.com/OpenGene/fastp) version 0.23.2.
