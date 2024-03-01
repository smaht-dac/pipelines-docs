---
layout: page
title: BQSR
menubar: docs_menu
tabs: short_read_paired_end_illumina_tabs
show_sidebar: false
---

# Base Quality Score Recalibration (BQSR)

In this final step, the pipeline recalibrates the base quality scores produced by the sequencing machine, correcting systematic (non-random) technical errors leading to over- or under-estimated base quality scores in the data. BQSR modeling is independently performed on groups of reads from different sequencer units (i.e., lanes), identified by the `PU` tag in the read groups.

## Base Quality Score Modeling and Recalibration

Pipeline command:

```text

sentieon driver -r reference.fasta
                -i deduped.bam
                --algo QualCal
                -k known_sites_SNP.vcf
                -k known_sites_INDEL.vcf
                recal_data.table

sentieon driver -r reference.fasta
                -i deduped.bam
                -q recal_data.table
                --algo ReadWriter
                recalibrated.bam

```

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in SAM format.

## Implementation with Sentieon

The implementation uses Sentieon QualCal algorithm to construct models of covariation based on the input data and a set of known variants. This process produces the recalibration table necessary for BQSR. The recalibration is then applied to the BAM file using the ReadWriter command. Currently, the pipeline is using Sentieon version 202308.01, corresponding to GATK versions 3.7, 3.8, 4.0, and 4.1. The algorithms are equivalent to BaseRecalibrator and ApplyBQSR algorithms in GATK.

*Note: To reduce run time (at the expense of accuracy), GATK4 disables the base quality score recalibration of indels when using default settings. Consequently, the Sentieon BAM output will contain BI/BD tags from the indel recalibration, which will be missing from the GATK4 BAM output when run with default settings.*

GATK equivalent command:

```text

gatk BaseRecalibrator -R reference.fasta
                      -I deduped.bam
                      --enable-baq
                      --known-sites known_sites_SNP.vcf
                      --known-sites known_sites_INDEL.vcf
                      -O recal_data.table

gatk ApplyBQSR -R reference.fasta
               -I deduped.bam
               -bqsr recal_data.table
               -O recalibrated.bam

```

## Source Code

All the relevant code can be accessed in the [GitHub repository](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_QualCal.sh)
