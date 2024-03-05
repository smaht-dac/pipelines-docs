---
layout: page
title: Alignment
menubar: docs_menu
tabs: long_read_pacbio-hifi_tabs
show_sidebar: false
---

# Alignment with pbmm2

For alignment, the pipeline uses pbmm2 on each unaligned BAM file. The software also sorts the reads by genomic coordinates and links the methylation tags, if present. An integrity check is then performed on the resulting BAM file.

## Aligning and Sorting

##### Align and sort reads

```text
pbmm2 align --sort --strip --unmapped reference.fasta unaligned.bam sorted.bam
```

Arguments:

- *-\-sort*: sort the aligned reads by genomic coordinates.
- *-\-strip*: remove extraneous tags if present in the input BAM file. Tags removed: `dq, dt, ip, iq, mq, pa, pc, pd, pe, pg, pm, pq, pt, pv, pw, px, sf, sq, st`.
- *-\-unmapped*: retain unmapped reads.

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in SAM format.

## Implementation with pbmm2

The pipeline is currently using pbmm2 version 1.13.0, which utilizes minimap2 version 2.26. It's important to note that pbmm2 sets some defaults that may differ from the standard minimap2.

Default set by pbmm2 for minimap2:

- Soft clipping is enabled with `-Y`.
- Long cigars for the `CG` tag are set using `-L`.
- X/= cigars are used instead of M with `--eqx`.
- Overlapping query intervals with repeated matches trimming are disabled.
- Secondary alignments are excluded with `--secondary=no`.

*Note: Due to multi-threading the output alignment ordering can differ between multiple runs with the same input parameters. The same can occur even with option --sort for records that align to the same target sequence, the same position within that target, and in the same orientation, which are the only fields that samtools sort uses.*
