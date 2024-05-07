
# Read Groups

A read group (`@RG`) is a unique identifier that group reads together, capturing relevant information about the sample and the sequencing process and technology, utilized by various downstream bioinformatics tools.

The relevant fields in defining a read group include:

- **ID (Identifier):** A unique identifier for the read group within the BAM file and across multiple BAM files used in the same dataset.
- **SM (Sample):** The sample to which the reads belong.
- **PL (Platform):** The technology used to sequence the reads (e.g., ONT).
- **PM (Platform Model):** The platform model reflecting the instrument series.
- **PU (Platform Unit):** A unique identifier for the sequencer unit used for sequencing.
- **LB (Library):** The library used to sequence the reads.
- **DS (Description):** Semantic information about the reads in the group, encoded as a semicolon-delimited list of “Key=Value” strings.
- **DT (Date/Time):** The date and time when the run was produced (ISO8601 date or date/time).
- **basecall_model:** The model used for base calling.

## Assigning Read Groups

The original read groups from the unaligned BAM files are linked and maintained in the corresponding alignment BAM files. In-house bash code that utilizes samtools replaces `SM` and `LB` information with the correct identifiers used by the portal, as follows:

- **SM:** `<sample name>`
- **LB:** `<sample name>.<library>`

E.g., in BAM file:

```text
@RG	ID:bcdb4058-3545-4c45-aea9-4159f1c2ca7d_dna_r10.4.1_e8.2_400bps_sup@v4.2.0	DT:2024-02-21T12:56:53.022625-06:00	DS:runid=bcdb4058-3545-4c45-aea9-4159f1c2ca7d	basecall_model=dna_r10.4.1_e8.2_400bps_sup@v4.2.0	LB:SMACUWVOKOZU.SMALI56YAYM5	PL:ONT	PM:3A	PU:PAW14872	al:unclassified SM:SMACUWVOKOZU
```

## Source Code

All the relevant code is accessible in the GitHub repository:

  - [ImportReadGroups_methylink.sh](https://github.com/smaht-dac/alignment-pipelines/blob/main/dockerfiles/methylink/ImportReadGroups_methylink.sh) [ImportReadGroups]