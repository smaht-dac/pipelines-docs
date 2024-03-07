---
layout: page
title: Overview
menubar: docs_menu
tabs: preprocessing_fastq_tabs
show_sidebar: false
---

# FASTQ Files Preprocessing

FASTQ files contain raw sequencing information, including nucleotide sequences and quality scores, and serve as the initial input for a majority of genomic analyses. However, the potential presence of artifacts, low-quality, or residual adapter sequences can significantly impact downstream analysis. Preprocessing steps, including quality trimming, adapter, and artifact removal, may be necessary to ensure data quality and meeting requirements for subsequent analyses.


## Key Preprocessing Steps

1. **polyG Artifacts Removal:** Removal of reads containing polyG artifacts from Illumina sequencing data.
