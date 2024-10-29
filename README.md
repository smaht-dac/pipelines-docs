# SMaHT Pipelines Documentation

Welcome to the documentation for SMaHT analysis pipelines and associated resources.

## CONTENTS

##### PREPROCESSING

- [**FASTQ Files**](/DOCS/PREPROCESSING/FASTQ_Files/0_Overview.md)

    - [polyG Artifacts Removal](/DOCS/PREPROCESSING/FASTQ_Files/1_polyG_Artifacts_Removal.md)

##### ALIGNMENT

- [**Short-Read Illumina, *Paired-End***](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/0_Overview.md)

    - [Alignment](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/1_Alignment.md)
    - [Read Groups](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/2_Read_Groups.md)
    - [Duplicate Reads](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/3_Duplicate_Reads.md)
    - [Local Realignment](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/4_Local_Realignment.md)
    - [Base Quality Score Recalibration](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/5_Base_Quality_Score_Recalibration.md)
    - [Hi-C](/DOCS/ALIGNMENT/Short-Read_Illumina_Paired-End/6_Hi-C.md)

- [**Long-Read PacBio HiFi**](/DOCS/ALIGNMENT/Long-Read_PacBio_HiFi/0_Overview.md)

    - [Alignment](/DOCS/ALIGNMENT/Long-Read_PacBio_HiFi/1_Alignment.md)
    - [Read Groups](/DOCS/ALIGNMENT/Long-Read_PacBio_HiFi/2_Read_Groups.md)
    - [Methylation and Tags](/DOCS/ALIGNMENT/Long-Read_PacBio_HiFi/3_Methylation_and_Tags.md)

- [**Long-Read Oxford Nanopore**](/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/0_Overview.md)

    - [Alignment](/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/1_Alignment.md)
    - [Read Groups](/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/2_Read_Groups.md)
    - [Methylation and Tags](/DOCS/ALIGNMENT/Long-Read_Oxford_Nanopore/3_Methylation_and_Tags.md)

##### ANALYSIS

- [**Short-Read RNA-seq, *Paired-End***](/DOCS/ANALYSIS/Short-Read_RNA-seq_Paired-End/0_Overview.md)

    - [Alignment](/DOCS/ANALYSIS/Short-Read_RNA-seq_Paired-End/1_Alignment.md)
    - [Duplicate Reads](/DOCS/ANALYSIS/Short-Read_RNA-seq_Paired-End/2_Duplicate_Reads.md)
    - [Transcript Quantification](/DOCS/ANALYSIS/Short-Read_RNA-seq_Paired-End/3_Transcript_Quantification.md)
    - [Gene Quantification](/DOCS/ANALYSIS/Short-Read_RNA-seq_Paired-End/4_Gene_Quantification.md)

##### REFERENCE FILES

- [**Genome Builds**](/DOCS/REFERENCE_FILES/Genome_Builds/0_Overview.md)

    - [Build GRCh38](/DOCS/REFERENCE_FILES/Genome_Builds/1_Build_GRCh38.md)

- [**Genome Annotations**](/DOCS/REFERENCE_FILES/Genome_Annotations/0_Overview.md)

    - [GENCODE](/DOCS/REFERENCE_FILES/Genome_Annotations/1_GENCODE.md)

- [**Variant Catalogs**](/DOCS/REFERENCE_FILES/Variant_Catalogs/0_Overview.md)

    - [Single Nucleotide Polymorphism Database](/DOCS/REFERENCE_FILES/Variant_Catalogs/1_dbSNP.md)
    - [Mills and 1000 Genomes Project](/DOCS/REFERENCE_FILES/Variant_Catalogs/2_Mills_and_1kGP.md)

- Software Specific

    - [Burrows-Wheeler Transform Index](/DOCS/REFERENCE_FILES/Software_Specific/0_BWT_Index.md)
    - [STAR Index](/DOCS/REFERENCE_FILES/Software_Specific/1_STAR_Index.md)
    - [RSEM Reference](/DOCS/REFERENCE_FILES/Software_Specific/2_RSEM_Reference.md)

##### [Release CHANGELOG](releases.md)