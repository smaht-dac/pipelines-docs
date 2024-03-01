---
layout: page
title: Read Groups
menubar: docs_menu
tabs: short_read_paired_end_illumina_tabs
show_sidebar: false
---

# Assign Read Groups

As the second step, the pipeline assigns the reads to unique read groups, representing identifiers that group reads together. A read group (`@RG`) captures relevant information about the sample and the sequencing process and technology, utilized by various downstream bioinformatics tools.

The relevant fields in defining a read group include:

- **ID (Identifier):** A unique identifier for the read group within the BAM file and across multiple BAM files used in the same dataset.
- **SM (Sample Name):** The sample to which the reads belong.
- **PL (Platform):** The technology used to sequence the reads. This tag is required if running Base Quality Score Recalibration (BQSR) to determine the correct error model (e.g., ILLUMINA).
- **PU (Platform Unit):** A unique identifier for the sequencer unit used for sequencing (i.e., the sequencing lane). This tag is required if running BQSR, as it models together all reads belonging to the same platform unit.
- **LB (Library):** The library used to sequence the reads. This tag is used in the process of marking or removing duplicate reads to determine groups that may contain duplicates, as duplicate reads need to belong to the same library.

## Generating Read Groups

To assign read groups, an in-house Python script is used. It can automatically generate read groups based on Illumina read names and handle multiple read groups in the same file (e.g., reads from multiple lanes are merged into a single file).

The read groups are assigned as follows:

- **ID:** `<sample name>.<instrument>_<run>_<flow cell>.<lane>`
- **SM:** `<sample name>`
- **PL:** `<platform>`
- **PU:** `<instrument>_<run>_<flow cell>.<lane>`
- **LB:** `<sample name>.<library>`

E.g., in BAM file:

```text

@RG ID:SMAHT1.ST-E00127_336_HJ7YHCCXX.8  SM:SMAHT1  PL:ILLUMINA  PU:ST-E00127_336_HJ7YHCCXX.8  LB:SMAHT1.HISEQ-LIB1

```

## Source Code

All the relevant code is accessible in the GitHub repository:

  - [AddReadGroups.py](https://github.com/smaht-dac/pipelines-scripts/blob/main/processing_scripts/AddReadGroups.py) [AddReadGroups]
