
# FASTQ Files

FASTQ files contain raw sequencing information, including nucleotide sequences and quality scores, and serve as the initial input for a majority of analyses. However, the presence of artifacts, low-quality, or residual adapter sequences can significantly impact downstream processing. Preprocessing steps, including quality trimming, adapter, and artifact removal, may be necessary to ensure data quality and meeting requirements for subsequent analyses.


## Key Preprocessing Steps

1. **polyG Artifacts Removal:** Removal of reads containing polyG artifacts from Illumina sequencing data.
