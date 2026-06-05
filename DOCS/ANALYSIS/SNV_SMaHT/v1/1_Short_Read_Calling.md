
# Short-Read Variant Calling

The pipeline detects candidate single-nucleotide variants (SNVs) from short-read sequencing data using three independent and complementary variant callers: Strelka2, Mutect2 (Sentieon TNhaplotyper2 implementation), and RUFUS.

All short-read sequencing libraries associated with a tissue sample are merged prior to variant calling and processed jointly. The combined dataset provides high-depth coverage (~300X) to increase sensitivity for low-allele fraction mosaic variants.

Each caller produces an independent set of candidate SNV calls that are retained for downstream merging and filtering.

## Strelka2 Variant Calling

##### Run Strelka2

```text
configureStrelkaSomaticWorkflow.py \
  --normalBam germline.bam \
  --tumorBam sample.bam \
  --referenceFasta reference.fasta \
  --callRegions regions.bed.gz \
  --runDir strelka_run
```

Strelka2 is run in tumor-normal mode.

*Note: `germline.bam` was generated from merged Broad GCC ST002-1D Illumina WGS datasets (SMAFINVNFEWN and SMAFIFO11KT3; 220X total coverage). This file is used as the non-matched normal for each other tissue sample.*

##### Distributed Mode

Variant calling is restricted to genomic intervals specified by an indexed BED file (`--callRegions`). These intervals are distributed across parallel jobs and machines to achieve full genome coverage.

## Mutect2 Variant Calling

##### Run Mutect2 (Sentieon TNhaplotyper2)

```text
sentieon driver -r reference.fasta \
  -i sample.cram \
  --algo TNhaplotyper2 \
  --tumor_sample sample \
  --germline_vcf population_af.vcf.gz \
  output.vcf.gz \
  --algo OrientationBias --tumor_sample sample output.priors \
  --algo ContaminationModel --tumor_sample sample -v population_af.vcf.gz output.contamination
```

The pipeline uses the Sentieon implementation of the Mutect2 algorithm (TNhaplotyper2). Sentieon provides an implementation of the Mutect2 that is algorithmically equivalent to the original GATK version while improving computational performance.

The algorithm is run in tumor-only mode.

During variant calling the pipeline also computes orientation bias and contamination metrics using the `OrientationBias` and `ContaminationModel` algorithms.

*Note: The population allele frequency resource used by Mutect2 is released by the Broad Institute as part of the GATK Best Practices. For additional details, see the gnomAD Population Allele Frequencies for Mutect2 documentation in the "Variant Catalogs" section.*

##### Distributed Mode

To improve scalability, the pipeline distributes the analysis across genomic intervals using shard definitions (`--shard`). Each interval contains a subset of genomic regions processed together within a single job. Intervals are executed independently across multiple machines to achieve full genome coverage.

Each interval also produces orientation bias and contamination model metrics that are merged during downstream processing.

##### Merge shards and filter variants

After all shards complete, the pipeline merges the intermediate outputs and performs final variant filtering.

```text
sentieon driver -r reference.fasta \
  --algo TNfilter \
  -v merged.vcf.gz \
  --tumor_sample sample \
  --contamination merged.contamination \
  --tumor_segments merged.segments \
  --orientation_priors merged.priors \
  filtered.vcf.gz
```

The filtering step applies Mutect2 statistical models to remove artifacts associated with sequencing errors, strand bias, and sample contamination.

## RUFUS Variant Calling

##### Run RUFUS

```text
runRufus.sh \
  -s sample.cram \
  -cr reference.fasta \
  -m 5 \
  -k 25 \
  -L -vs \
  -e population_hash.Jhash \
  -e control_hash.Jhash
```

Arguments:

- *-m*: minimum k-mer depth required to support a candidate variant.
- *-k*: k-mer length used during variant detection.
- *-L*: report Mosaic/Low Frequency/Somatic variants.
- *-vs*: use very short assembly methods.

RUFUS detects candidate variants using a *k-mer based strategy* that identifies sequence differences directly from sequencing reads, instead of relying on alignment-based variant discovery.

##### Distributed Mode

To improve scalability, the pipeline distributes the analysis across genomic intervals using shard definitions. Each shard contains multiple genomic regions that are processed independently using the `-R` option.

Regions are processed in parallel, allowing multiple RUFUS jobs to run simultaneously on the same machine within each shard. Shards are distributed across multiple machines to achieve full genome coverage.

##### Reference and Population Hash Filtering

During variant detection, RUFUS compares observed k-mers against two sets of precomputed hash tables:

- **Control hashes** derived from the reference genome
- **Population hashes** derived from the 1000 Genomes dataset (KG1)

These hashes allow RUFUS to efficiently filter common sequences and focus on candidate variant signals.

For more information on the hash tables, please refer to the original software documentation [here](https://github.com/stefinfection/RUFUS).

*Note: For mitochondrial regions, population hashes are excluded because population hash tables are not available for those intervals.*

## Software Version

The Sentieon implementation replicates the original Mutect2 algorithm. The pipeline uses Sentieon version 202503.01, corresponding to GATK version 4.2.0.0. Strelka2 version 2.9.10 and RUFUS version d1.1.7 are used.

## Source Code

All the relevant code can be accessed in the GitHub repository:

- [strelka2_region.sh](https://github.com/smaht-dac/calling-pipelines/blob/main/dockerfiles/strelka2/strelka2_region.sh) [Strelka2]
- [sentieon_TNhaplotyper2_wOrientationBias_ContaminationModel.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/tnhaplotyper2/sentieon_TNhaplotyper2_wOrientationBias_ContaminationModel.sh) [TNhaplotyper2]
- [sentieon_merge_TNfilter.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/tnhaplotyper2/sentieon_merge_TNfilter.sh) [TNfilter]
- [run_rufus.sh](https://github.com/smaht-dac/calling-pipelines/blob/main/dockerfiles/rufus/run_rufus.sh) [RUFUS]

---

<!-- This section relies on the html links generated by GitHub Pages 
and will not render correctly in Markdown -->
<div style="text-align: right">
    <a href="/pipelines-docs/"> Home </a> -
    <a href="0_Overview.html"> Overview </a> -
    <a> <b> Short-Read Calling </b> </a> -
    <a href="2_Long_Read_Calling.html"> Long-Read Calling </a> -
    <a href="3_Calls_Merging.html"> Calls Merging </a> -
    <a href="4_Filtering.html"> Filtering </a> -
    <a href="5_Cross_Validation.html"> Cross-Technology Validation </a> -
    <a href="6_Donor_Level_Refinement.html"> Donor-Level Refinement </a> -
    <a href="7_Confidence_Designation.html"> Confidence Designation </a>
</div>