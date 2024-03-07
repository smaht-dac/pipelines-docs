---
layout: page
title: GRCh38
menubar: docs_menu
tabs: reference_genomes_tabs
show_sidebar: false
---

# GRCh38 Genome Build

GRCh38, or hg38, serves as a standardized representation of DNA sequences for various alignment and analysis pipelines. The specific version in use is GCA_000001405.15 no_alt_analysis_set, accessible for download [here](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.15_GRCh38/seqs_for_alignment_pipelines.ucsc_ids/) in the following files:

- **Sequences FASTA:** GCA_000001405.15_GRCh38_no_alt_analysis_set.fna.gz
- **FM Index:** GCA_000001405.15_GRCh38_no_alt_analysis_set.fna.bwa_index.tar.gz

This version excludes ALT contigs and Human decoy sequences from hs38d1 (GCA_000786075.2), and includes the following sequences:

- Chromosomes from the GRCh38 Primary Assembly unit.
- Mitochondrial genome from the GRCh38 non-nuclear assembly unit.
- Unlocalized scaffolds from the GRCh38 Primary Assembly unit.
- Unplaced scaffolds from the GRCh38 Primary Assembly unit.
- Epstein-Barr virus (EBV) sequence.

*Note: The two PAR regions on chrY have been hard-masked with Ns, and the chromosome Y sequence is not identical to the GenBank sequence but shares the same coordinates. Similarly, duplicate copies of centromeric arrays and WGS on chromosomes 5, 14, 19, 21 & 22 have been hard-masked with Ns.*

*Note: The EBV sequence is not part of the genome assembly but is included in the analysis set for aligning reads often present in sequencing samples.*
