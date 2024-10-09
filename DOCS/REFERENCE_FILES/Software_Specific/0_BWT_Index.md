
# Burrows-Wheeler Transform (BWT) Index

The BWT index is generated from the Genome Reference Consortium Human Build 38 (GRCh38), specifically version GCA_000001405.15 using the no_alt_analysis_set. 

For more detailed information about this reference genome, please refer to the build GRCh38 documentation under “Genome Builds” section.

## Generating BWT Index

The index is generated using BWA indexing algorithm.

##### Generate BWT index

```text
sentieon bwa index reference.fasta
```

## Implementation with Sentieon

Sentieon implementation replicates the original [BWA](https://github.com/lh3/bwa) code. The current reference index was generated using Sentieon version 202308.01, corresponding to BWA version 0.7.17.

---

<!-- This section relies on the html links generated by GitHub Pages 
and will not render correctly in Markdown -->
<div style="text-align: right">
    <a href="/pipelines-docs/"> Home </a> -
    <a> <b> BWT Index </b> </a> -
    <a href="1_STAR_Index.html"> STAR Index </a> -
    <a href="2_RSEM_Reference.html"> RSEM Reference </a>
</div>