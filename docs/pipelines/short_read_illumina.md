---
layout: page
title: Short-Read Illumina
menubar: docs_menu
tabs: short_read_illumina_tabs
show_sidebar: false
---

# Alignment Pipeline for Paired-End Short-Read Data

The alignment pipeline for paired-end short-read data is designed to run per-sample and per-library. It begins with multiple paired-end sets of FASTQ files generated from each sequencing lane. The pipeline is optimized for distributed processing.

## Pipeline Steps:

1. **Alignment (BWA-MEM):** This initial step aligns the raw reads to the reference genome.
2. **Adding Read Groups:** This step assigns reads to different groups, providing essential information for subsequent steps.
3. **Marking Duplicates:** This step identifies and labels duplicate reads originating during library preparation or as sequencing artifacts.
4. **Base Quality Score Recalibration:** This step recalibrates base quality scores, including indels, and produces the final analysis-ready BAM file.

To address the need for scalability, especially with deep whole-genome sequencing data (WGS), we've implemented a pipeline that utilizes a more efficient software implementation from Sentieon. This implementation follows the Genome Analysis Toolkit (GATK) Best Practices.

Sentieon, provides a comprehensive toolkit (DNASeq) that replicates the original BWA and GATK algorithms. It significantly enhances computational efficiency and speed. We also leverage Sentieon’s distributed mode to streamline the processing of the large data volumes involved in handling high-coverage genome data.

![Short-Read Illumina Alignment Flow](images/short_read_distributed.png)
