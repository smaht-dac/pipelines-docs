
# Build GRCh38

The Genome Reference Consortium Human Build 38 (GRCh38), or hg38, serves as a standardized representation of DNA sequences for various alignment and analysis pipelines.

The specific version in use is GCA_000001405.15 no_alt_analysis_set, accessible for download [here](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.15_GRCh38/seqs_for_alignment_pipelines.ucsc_ids/) in the following files:

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

## Generating FASTA Index Files

##### Decompress the FNA file 

```text
gunzip GCA_000001405.15_GRCh38_no_alt_analysis_set.fna.gz
```

##### Change the file extension to `.fasta `

```text
mv GCA_000001405.15_GRCh38_no_alt_analysis_set.fna GCA_000001405.15_GRCh38_no_alt_analysis_set.fasta
```

##### Create the `faidx` index (requires [samtools](https://www.htslib.org/))

```text
samtools faidx GCA_000001405.15_GRCh38_no_alt_analysis_set.fasta
```

##### Create the sequence dictionary (requires [picard](https://broadinstitute.github.io/picard/))

```text
java -jar picard.jar CreateSequenceDictionary R=GCA_000001405.15_GRCh38_no_alt_analysis_set.fasta
```