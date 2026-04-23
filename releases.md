
# Release 0.3.1

This release includes the following modules:

* **shared-pipelines** ()
* **alignment-pipelines** ()
* **sentieon-pipelines** ()
* **qc-pipelines** ()
* **rnaseq-pipelines** ()
* **calling-pipelines** ()
* **smaht-pipeline-utils** ()

## Pipelines

| Pipeline                                  | Category                                | Status/Changes                                         |
|-------------------------------------------|-----------------------------------------|--------------------------------------------------------|
| **Short-Read Illumina, _Paired-End_**     | Alignment                               |+ Added conversion to CRAM                                |
| **Long-Read PacBio HiFi**                 | Alignment                               | + Added conversion to CRAM                                |
| **Long-Read Oxford Nanopore**             | Alignment                               | + Added conversion to CRAM                                |
| **Short-Read RNA-seq, _Paired-End_**      | Alignment, Quality Control, Analysis    | No change *since 0.3.0*                                |
| **Long-Read RNA-seq, _PacBio Kinnex_**    | Alignment, Quality Control, Analysis    | No change *since 0.2.0*                                |
| **Germline Variant Calling, _DNAscope Hybrid_**    | Variant Calling    | *New pipeline added*                            |
| **SMaHT Single-Nucleotide Variant Calling**        | Variant Calling    | *New pipeline added*                            |
| **Short-Read Aligned, _Paired-End_ (BAM)**| Quality Control                         | No change *since 0.1.0*                                |
| **Long-Read Aligned (BAM)**               | Quality Control                         | No change *since 0.1.0*                                |
| **Short-Read Unaligned (FASTQ)**          | Quality Control                         | No change *since 0.1.0*                                |
| **Long-Read Unaligned (FASTQ, uBAM)**     | Quality Control                         | No change *since 0.2.0*                                |
| **Sample Identity Check**                 | Quality Control                         | No change *since 0.2.0*                                |

## Software

| Software            | Software Version | Category                   | Algorithm                              | Source               |
|---------------------|------------------|----------------------------|----------------------------------------|----------------------|
| **BWA**             | 0.7.17           | Aligner                    | BWA-MEM                                | Sentieon 202308.01   |
| **pbmm2**           | 1.13.0           | Aligner                    | -                                      | -                    |
| **minimap2**        | 2.26             | Aligner                    | -                                      | Sentieon 202308.01   |
| **STAR**            | 2.7.10b          | Aligner                    | -                                      | Sentieon 202308.01   |
| **Picard**          | 3.0.0            | Utilities                  | -                                      | -                    |
| **Picard**          | 2.9.0            | Utilities                  | MarkDuplicates                         | Sentieon 202308.01   |
| **GATK**            | 4.4.0.0          | Utilities                  | -                                      | -                    |
| **GATK**            | 3.8              | Utilities                  | RealignerTargetCreator, IndelRealigner | Sentieon 202308.01   |
| **GATK**            | 4.1              | Utilities                  | BaseRecalibrator, ApplyBQSR            | Sentieon 202308.01   |
| **methylink**       | 0.6.0            | Utilities                  | -                                      | -                    |
| **Samtools**        | 1.17             | Utilities                  | -                                      | -                    |
| **Bcftools**        | 1.23             | Utilities                  | -                                      | -                    |
| **Bedtools**        | 2.31.1           | Utilities                  | -                                      | -                    |
| **fastp**           | 0.23.2           | Utilities                  | -                                      | -                    |
| **pbtk**            | 3.4.0            | Utilities                  | -                                      | -                    |
| **VEP**             | 114              | Utilities                  | -                                      | -                    |
| **minipileup**      | 1.4b             | Utilities                  | -                                      | -                    |
| **granite-suite**   | 0.5.0            | Utilities, Variant Caller  | -                                      | -                    |
| **FastQC**          | v0.12.0          | Quality control            | -                                      | -                    |
| **NanoPlot**        | 1.44.1           | Quality control            | -                                      | -                    |
| **VerifyBamID2**    | 2.0.1            | Quality control            | -                                      | -                    |
| **Kraken2**         | v2.1.3           | Quality control            | -                                      | -                    |
| **mosdepth**        | v0.3.9           | Quality control            | -                                      | -                    |
| **Somalier**        | v0.2.19          | Quality control            | -                                      | -                    |
| **RSEM**            | v1.3.3           | Analysis                   | -                                      | -                    |
| **RNA-SeQC**        | 2.4.2            | Analysis, Quality control  | -                                      | -                    |
| **IsoSeq**          | 4.2.0            | Analysis, Quality control  | -                                      | -                    |
| **Pigeon**          | 1.3.0            | Analysis, Quality control  | -                                      | -                    |
| **Mutect2**         | 4.2.0.0          | Variant Caller             | TNhaplotyper2                          | Sentieon 202503.01   |
| **DNAscope Hybrid** | 1.5.0            | Variant Caller             | -                                      | Sentieon 202503.01   |
| **Strelka2**        | 2.9.10           | Variant Caller             | -                                      | -                    |
| **longcallD**       | 0.0.8            | Variant Caller             | -                                      | -                    |
| **RUFUS**           | d1.1.7           | Variant Caller             | -                                      | -                    |

<sub> _Software_: the tool whose behavior the pipeline matches. _Algorithm_: the specific algorithm, driver, or subcommand invoked. Source_: source for the software, when it's not the original release. </sub>

# Older Releases

- [0.3.0](/RELEASES/CODEBASE/0_3_0.md)
- [0.2.0](/RELEASES/CODEBASE/0_2_0.md)
- [0.1.0](/RELEASES/CODEBASE/0_1_0.md)
- [0.0.1](/RELEASES/CODEBASE/0_0_1.md)
