
# Release 0.1.0

This release includes the following modules:

- **shared-pipelines** (Commit Hash: )
- **alignment-pipelines** (Commit Hash: )
- **sentieon-pipelines** (Commit Hash: )
- **qc-pipelines** (Commit Hash: )
- **rnaseq-pipelines** (Commit Hash: )
- **smaht-pipeline-utils** (Commit Hash: )

## Pipelines

| Pipeline                                  | Category                                | Status/Changes                                         |
|-------------------------------------------|-----------------------------------------|--------------------------------------------------------|
| **Short-Read Illumina, _Paired-End_**     | Alignment                               | + Added local indel realignment<br>+ Added support for DRAGEN read names with UMI information |
| **Long-Read PacBio HiFi**                 | Alignment                               | No change *since 0.0.1*                                |
| **Long-Read Oxford Nanopore**             | Alignment                               | No change *since 0.0.1*                                |
| **Short-Read RNA-seq, _Paired-End_**      | Alignment, Quality Control, Analysis    | *New pipeline added*                                   |
| **Short-Read Aligned, _Paired-End_ (BAM)**| Quality Control                         | + Added VerifyBamID<br>+ Added mosdepth                |
| **Long-Read Aligned (BAM)**               | Quality Control                         | + Added VerifyBamID<br>+ Added mosdepth                |
| **Short-Read Unaligned (FASTQ)**          | Quality Control                         | + Added Kraken2                                        |
| **Long-Read Unaligned (FASTQ, uBAM)**     | Quality Control                         | *New pipeline added*                                   |

## Software

| Software         | Category                      | Version      | Commands                                         | Source               |
|------------------|-------------------------------|--------------|--------------------------------------------------|----------------------|
| **BWA**          | Aligner                       | 0.7.17       | -                                                | Sentieon 202308.01   |
| **pbmm2**        | Aligner                       | 1.13.0       | -                                                | -                    |
| **minimap2**     | Aligner                       | 2.26         | -                                                | Sentieon 202308.01   |
| **STAR**         | Aligner                       | 2.7.10b      | -                                                | Sentieon 202308.01   |
| **Picard**       | Utilities                     | 3.0.0        | -                                                | -                    |
| **Picard**       | Utilities                     | 2.9.0        | MarkDuplicates                                   | Sentieon 202308.01   |
| **GATK**         | Utilities                     | 4.4.0.0      | -                                                | -                    |
| **GATK**         | Utilities                     | 3.8          | RealignerTargetCreator, IndelRealigner           | Sentieon 202308.01   |
| **GATK**         | Utilities                     | 4.1          | BaseRecalibrator, ApplyBQSR                      | Sentieon 202308.01   |
| **methylink**    | Utilities                     | 0.6.0        | -                                                | -                    |
| **Samtools**     | Utilities                     | 1.17         | -                                                | -                    |
| **Bcftools**     | Utilities                     | 1.17         | -                                                | -                    |
| **fastp**        | Utilities                     | 0.23.2       | -                                                | -                    |
| **FastQC**       | Quality control               | v0.12.0      | -                                                | -                    |
| **NanoPlot**     | Quality control               | 1.42.0       | -                                                | -                    |
| **VerifyBamID2** | Quality control               | 2.0.1        | -                                                | -                    |
| **Kraken2**      | Quality control               | v2.1.3       | -                                                | -                    |
| **mosdepth**     | Quality control               | v0.3.9       | -                                                | -                    |
| **RSEM**         | Analysis                      | v1.3.3       | -                                                | -                    |
| **RNA-SeQC**     | Analysis, Quality control     | 2.4.2        | -                                                | -                    |

<sub>_Commands_: subset of commands used from the corresponding version; _Source_: source for the software, when it's not the original release</sub>

# Older Releases

- [0.0.1](/RELEASES/CODEBASE/0_0_1.md)