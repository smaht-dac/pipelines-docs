---
layout: page
title: Methylation, Fiber-seq
menubar: docs_menu
tabs: short_read_paired_end_illumina_tabs
show_sidebar: false
---

# Methylation Tags Linking, Fiber-seq

Fiber-seq[^1] is a chromatin mapping technique that employs methyltransferases to create a stencil, mapping chromatin fibers onto the underlying DNA template. The method offers high-resolution insights into chromatin structure, at nearly single-molecule level, facilitating the monitoring of nucleosome positions.

PacBio HiFi data are processed through [fibertools-rs](https://github.com/fiberseq/fibertools-rs) to generate unaligned BAM files. fibertools-rs adds additional information to the files creating tags that are linked by pbmm2 during alignment.

Fiber-seq generates the following tags:

- **MM (mCpG and m6A Methylation Positions):** Positions of mCpG and m6A methylation along the read.
- **ML (Methylation Precision Values):** Precision values for each mCpG and m6A methylation call.
- **ns (Nucleosome Start Positions):** Start positions of identified nucleosomes along the read relative to the first base.
- **nl (Nucleosome Lengths):** Lengths of each nucleosome along the read.
- **as (Methyltransferase Accessible Patch Start Positions):** Start positions of identified methyltransferase accessible patches (MSP) along the read.
- **al (MSP Lengths):** Lengths of each MSP along the read.

---

[^1]: *Andrew B. Stergachis et al.* Single-molecule regulatory architectures captured by chromatin fiber sequencing. *Science 368, 1449-1454(2020).* doi: 10.1126/science.aaz1646.