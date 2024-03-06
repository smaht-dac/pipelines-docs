---
layout: page
title: Read Groups
menubar: docs_menu
tabs: long_read_pacbio-hifi_tabs
show_sidebar: false
---

# Read Groups Assignment

A read group (`@RG`) is a unique identifier that group reads together, capturing relevant information about the sample and the sequencing process and technology, utilized by various downstream bioinformatics tools.

The relevant fields in defining a read group include:

- **ID (Identifier):** A unique identifier for the read group within the BAM file and across multiple BAM files used in the same dataset.
- **SM (Sample):** The sample to which the reads belong.
- **PL (Platform):** The technology used to sequence the reads (e.g., PACBIO).
- **PM (Platform Model):** The platform model reflecting the instrument series (e.g., REVIO, ASTRO, SEQUEL, RS).
- **PU (Platform Unit):** A unique identifier for the sequencer unit used for sequencing (i.e., PacBio movie name).
- **LB (Library):** The library used to sequence the reads.
- **DS (Description):** Semantic information about the reads in the group, encoded as a semicolon-delimited list of “Key=Value” strings.

##### Mandatory Description (DS) Information

| Key               | Value Specification                                                                | Example     |
|-------------------|------------------------------------------------------------------------------------|-------------|
| READTYPE          | One of ZMW, HQREGION, SUBREAD, CCS, SCRAP, or UNKNOWN                              | CCS         |
| BINDINGKIT        | Binding kit part number                                                            | 102-739-100 |
| SEQUENCINGKIT     | Sequencing kit part number                                                         | 102-118-800 |
| BASECALLERVERSION | Basecaller version number                                                          | 5.0         |
| FRAMERATEHZ       | Frame rate in Hz                                                                   | 100         |
| CONTROL           | TRUE if reads are classified as spike-in controls, otherwise CONTROL key is absent | TRUE        |

##### Optional Description (DS) Information

| Key            | Value Specification                                                                        | Example                                |
|----------------|--------------------------------------------------------------------------------------------|----------------------------------------|
| BarcodeFile    | Name of the FASTA file containing the sequences of the barcodes used                       | m84046_230828_225743_s2.barcodes.fasta |
| BarcodeHash    | The MD5 hash of the contents of the barcoding sequence file                                | e7c4279103df8c8de7036efdbdca9008       |
| BarcodeCount   | The number of barcode sequences in the barcode file                                        | 113                                    |
| BarcodeMode    | Experimental design of the barcodes. Must be Symmetric/Asymmetric/Tailed or None Symmetric | Symmetric                              |
| BarcodeQuality | The type of value encoded by the bq tag. Must be Score/Probability/None                    | Score                                  |

## Assigning Read Groups

During the alignment process using pbmm2, the original read groups from the unaligned BAM files are linked and maintained in the corresponding alignment BAM files. In-house bash code that utilizes samtools replaces `SM` and `LB` information with the correct identifiers used by the portal, as follows:

- **SM:** `<sample name>`
- **LB:** `<sample name>.<library>`

E.g., in BAM file:

```text
@RG	ID:f115ea06/25--25	PL:PACBIO	DS:READTYPE=CCS;Ipd:Frames=ip;PulseWidth:Frames=pw;BINDINGKIT=102-739-100;SEQUENCINGKIT=102-118-800;BASECALLERVERSION=5.0;FRAMERATEHZ=100.000000;BarcodeFile=metadata/m84046_230828_225743_s2.barcodes.fasta;BarcodeHash=e7c4279103df8c8de7036efdbdca9008;BarcodeCount=113;BarcodeMode=Symmetric;BarcodeQuality=Score	LB:SMACUBS146SV.SMALIRA4HLNS	PU:m84046_230828_225743_s2	SM:SMACUBS146SV	PM:REVIO	BC:ACGCACGTACGAGTAT	CM:R/P1-C1/5.0-25M
```

## Source Code

All the relevant code is accessible in the GitHub repository:

  - [ReplaceReadGroups.sh](https://github.com/smaht-dac/pipelines-scripts/blob/main/processing_scripts/ReplaceReadGroups.sh) [ReplaceReadGroups]
