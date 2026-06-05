
# mSNV Calling and Filtering Pipeline — SmahtSNV 2.0.0

## Updates in SmahtSNV 2.0.0

The following changes have been introduced relative to the previous pipeline version (v1.0.0):

1. **Core-specific variant tracking**: Each GCC sequencing core is tracked independently throughout the pipeline. The final VCF is multi-sample, with one column per core recording per-core read counts, variant allele fractions, and caller information.

2. **CrossCore evidence label**: A fourth cross-evidence flag has been added. `CrossCore` is set when a variant is independently detected and supported in 2 or more sequencing cores from the same tissue (see [Cross-Evidence Classification](#cross-evidence-classification)). It complements the existing CrossTech, CrossCaller, and CrossTissue labels.

3. **Revised confidence system — PASS / LowEvidence / EvidenceScore**: The previous three-class system (HighConf / LowConf / LikelyArtifact) has been replaced. `EvidenceScore` is an integer INFO field (0–4) counting how many Cross* flags are present. Variants with at least one Cross* flag receive a FILTER of `PASS` (EvidenceScore ≥ 1); variants with no corroborating evidence receive `LowEvidence` (EvidenceScore = 0). Downstream analyses can apply additional stratification using EvidenceScore directly.

4. **Updated CrossTissue reporting**: Tissues from the same donor with short-read support for a variant are now listed in the `SR_TISSUE_PRESENCE` INFO field (replaces the previous `TISSUE_SR_VAFs` field).

---

## Overview

The SMaHT mosaic SNV pipeline (SmahtSNV 2.0.0) integrates four somatic SNV callers — three short-read-based and one long-read-based — followed by hierarchical filtering and multi-axis cross-evidence validation to generate high-confidence mosaic SNV calls. Both merged sequencing libraries from multiple genome centers (GCCs) as well as core-specific libraries for each donor are provided as high-depth input (~300× combined, or ~150x core-specific short-read coverage) to each caller. Starting in v2.0.0, each sequencing core is tracked independently, and the final output is a multi-sample VCF with one column per core.

### Key Pipeline Steps

Calling → Short-read filtering → Long-read validation → Cross-evidence tagging → Confidence assignment

---

## Somatic SNV Callers

All sequencing libraries from multiple GCCs for each donor are provided as high-depth input to each caller. PacBio data is also provided for long-read calling when available.

### Short-Read Based Callers

#### TNHaplotyper2

TNHaplotyper2 is run in tumor-only mode with default parameters.

#### Strelka2

Strelka2 is the only caller run in a paired mode. Because SMaHT investigates normal tissues rather than tumor-normal pairs, Strelka2 uses a fixed benchmarking tissue (ST002-1D, ~220×) as a pseudo-normal comparator for all production tissues.

#### RUFUS

RUFUS is run in single-sample mode and uses a panel of normals approach to filter false positives.

### Long-Read Based Callers

#### longcallD

Though longcallD supports both PacBio and ONT run modes, SNV calling uses only the PacBio mode. Higher false-positive rates were observed in ONT-based SNV calls, so ONT longcallD output is excluded from the candidate set.

---

## Short-Read Based Filtering

 1. Retain only variants marked PASS by the originating caller, followed by left-alignment, atomization of multiallelic sites, and normalization using bcftools. Applied independently per caller and per core.

 2. Merge all caller VCFs into a single per-tissue VCF. Two INFO fields are annotated at this step: `CALLERS` records the union of all callers that identified the variant across all cores; `CORE_CALLS` records the per-core caller breakdown (e.g. `001C1:Strelka2,RUFUS|001A3:Strelka2`).

 3. Remove variants present in the donor-matched DNAScope Hybrid germline call set. This removes inherited variants and any variant the germline caller confidently identified as constitutional.

 4. Exclude variants located within ±50 bp of another variant in the same sample. Clustered variants are a common artifact signature and this window matches the default filter used by TNHaplotyper2.

 5. Remove variants in regions with canonically problematic read alignments:
    - Centromeres
    - Segmental duplications
    - Simple repeats and satellites
    - Any variant within 5 bp of the Mills et al. set of 1000 Genomes-based indels

 6. Remove variants present in the Panel of Errors (POE), a set of recurrent artifactual sites derived from BSMN and 1000 Genomes normal sample data.

 7. Annotate all surviving variants with Ensembl VEP, adding functional consequence and gnomAD v4.1 population allele frequencies.

 8. Exclude variants with a gnomAD v4.1 `grpmax_joint_AF > 0.001` to minimize the inclusion of rare germline polymorphisms.

*Note: SNVs and indels are separated after step 8. Only SNVs proceed through the long-read filtering steps below.*

---

## Long-Read Based Filtering

 1. **Read support evaluation.** Pileup is collected at each surviving variant position across all short-read, PacBio, and ONT CRAMs for the donor. A binomial test (Poisson approximation, sequencing error rate 0.1%, p < 0.01) evaluates whether the observed number of alternate reads at a site is consistent with background sequencing error. Variants with sufficient short-read support alone, or with support corroborated across both short-read and tissue-matched PacBio data, proceed. At typical SMaHT sequencing depths (~300× short-read coverage), this threshold corresponds to roughly 2–3 supporting reads. Variants failing all read-support tests are removed.

 2. **Germline deviation check.** Variants passing the read-support evaluation are tested against a heterozygous germline model (expected VAF ≈ 0.5) using a two-sided binomial test. This check is applied independently to three data sources:
    - Short-read data pooled at the tissue level
    - PacBio data pooled at the donor level (all tissues combined)
    - ONT data pooled at the donor level
    Variants approaching germline VAF on any platform are removed. The minimum p-value across platforms is reported as `GERMLINE_PVAL`; per-platform values are available in `GERMLINE_PVAL_SR`, `GERMLINE_PVAL_PB`, and `GERMLINE_PVAL_ONT`.

 3. **PacBio read-backed phasing.** For each remaining candidate, PacBio reads are phased against the nearest heterozygous germline SNV within ±5 kb (from the DNAScope Hybrid germline call set) to assess haplotypic consistency. Variants for which no heterozygous germline SNV is available within ±5 kb pass this step and are labeled `UNABLE_TO_PHASE`.

---

## Cross-Evidence Classification

After long-read validation, four independent axes of evidence are evaluated and applied as Boolean INFO flags. All four labels can co-occur in any combination.

### CrossTech

The variant is supported by both short-read sequencing and tissue-matched PacBio long-read sequencing, with each platform independently exceeding a coverage-scaled read count threshold (Poisson-derived, error rate 0.1%, p < 0.01). Agreement between two sequencing technologies with fundamentally different error profiles — PCR-based Illumina and single-molecule PacBio — provides strong evidence against a technology-specific artifact.

### CrossCaller

The variant was independently identified by 2 or more distinct variant callers, across all sequencing cores considered together. Callers in different cores count toward this threshold. Agreement between algorithms with different underlying error models and calling strategies argues against a caller-specific artifact. The full set of supporting callers is recorded in the `CALLERS` INFO field.

### CrossCore

The variant passes the per-core read support gate in 2 or more sequencing cores from the same tissue. Each core is evaluated independently; a core contributes to this flag only if it originally called the variant and its read counts meet the coverage-appropriate threshold. Detection across physically distinct tissue sections prepared and sequenced independently argues against a section-specific or library-preparation artifact.

### CrossTissue

Short-read pileup at this position clears the read-support threshold (p < 1×10⁻⁵, sequencing error rate 0.1%) in 2 or more tissues from the same donor. The IDs of all supporting tissues are listed in the `SR_TISSUE_PRESENCE` INFO field. A variant present in multiple anatomically distinct tissues is more likely to have arisen early in development and is less likely to be a tissue-preparation artifact compared to a variant detected in only one tissue.

---

## Final Confidence Designation

The final confidence designations are either `PASS` or `LowEvidence` and are found in the `FILTER` field of the output VCF. An `EvidenceScore` integer INFO field records the number of Cross* flags present.

### EvidenceScore

```
EvidenceScore = CrossTech + CrossCaller + CrossCore + CrossTissue
```

Each flag contributes 1 if present, 0 if absent, giving a score of 0–4. This field is preserved in the final VCF to allow downstream analyses to apply additional stratification — for example, requiring EvidenceScore ≥ 2 for the most stringent call sets, or requiring CrossTech specifically for long-read-confirmed calls.

### PASS

Variants with EvidenceScore ≥ 1 receive a FILTER of `PASS`. These variants have been corroborated by at least one independent axis of evidence: a second sequencing technology, a second caller, a second core, or a second tissue. The specific combination of flags is retained in the INFO fields.

### LowEvidence

Variants with EvidenceScore = 0 receive a FILTER of `LowEvidence`. These variants cleared all hard filters (steps 1–8) and passed the statistical read-support and germline checks, but were observed in only a single technology, by a single caller, in a single core, and in a single tissue. They represent somatic candidates that cannot be distinguished from rare artifacts on the basis of corroboration alone. They are retained in the output for completeness but should be treated with caution, particularly in tissues without PacBio coverage where long-read corroboration is unavailable.
