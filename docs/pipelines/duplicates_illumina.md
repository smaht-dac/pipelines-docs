---
layout: page
title: Duplicates
menubar: docs_menu
tabs: short_read_paired_end_illumina_tabs
show_sidebar: false
---

# Duplicates Marking

In this step, the pipeline marks duplicate reads. Duplicate reads are sequencing artifacts that originate during library preparation and sequencing runs. Duplicate reads are evaluated per-library using the `LB` tag in the read groups.

The pipeline does not remove the duplicate reads that are tagged directly in the BAM file.

## Detecting and Marking Duplicates

#### Detect duplicate reads

```text
sentieon driver -i sorted.bam
                --algo LocusCollector
                --fun score_info score.txt
```

#### Mark duplicate reads

```text
sentieon driver -i sorted.bam
                --algo Dedup
                --optical_dup_pix_dist 2500
                --score_info score.txt
                deduped.bam
```

Arguments:

- *-optical_dup_pix_dist*: maximum offset between two duplicate clusters to consider them optical duplicates. For structured flow cells (NovaSeq, HiSeq 4000, X), the pipeline currently uses 2500.

*Note: The actual implementation of the above command in the pipeline is more complex to support distributed execution, but functionally equivalent.*

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in SAM format.

## Implementation with Sentieon

The pipeline implementation uses Sentieon LocusCollector to calculate duplicate metrics per library and the Dedup algorithm to mark duplicate reads in the BAM file. Currently, the pipeline is using Sentieon version 202308.01, corresponding to Picard 2.9.0. Both algorithms combined are equivalent to the MarkDuplicates algorithm in Picard.

#### Detect and mark duplicate reads (Picard equivalent)

```text
java -jar picard.jar MarkDuplicates
      INPUT=sorted.bam
      OPTICAL_DUPLICATE_PIXEL_DISTANCE=2500
      OUTPUT=deduped.bam
```

## Source Code

All the relevant code can be accessed in the GitHub repository:

  - [sentieon_LocusCollector.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_LocusCollector.sh) [LocusCollector]
  - [sentieon_LocusCollector_apply.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_LocusCollector_apply.sh) [Dedup]
