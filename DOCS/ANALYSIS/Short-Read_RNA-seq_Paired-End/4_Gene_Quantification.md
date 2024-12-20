
# Gene Quantification

The pipeline uses RNA-SeQC to quantify gene expression levels from RNA-seq data. It accepts alignments to the reference genome generated by STAR, with reads sorted by genomic coordinates. The software also compute key quality metrics, such as yield, alignment and duplication rates, transcript coverage, and RNA integrity scores.

## Quantifying Gene Expression

##### Gene expression quantification

```text
rnaseqc collapsed_gene_model.gtf \
        deduped.bam \
        . \
        -s OUT \
        --stranded rf
```

*Note: The above command assumes a paired-end, RF/fr-firststrand stranded protocol. The collapsed gene model was generate using GENCODE comprehensive gene annotations. For more detailed information please refer to the GENCODE documentation under “Genome Annotations” section.*

## Implementation with RNA-SeQC

The pipeline implements [RNA-SeQC](https://github.com/getzlab/rnaseqc) version 2.4.2. The software call is executed through a Python wrapper.

## Source Code

All the relevant code can be accessed in the GitHub repository:

  - [run_rnaseqc.py](https://github.com/smaht-dac/rnaseq-pipelines/blob/main/dockerfiles/gtex_v10/src/run_rnaseqc.py)<sup><sub>1</sub></sup> [RNA-SeQC]

<sub><b>1</b>: *Original author: Francois Aguet, Modified by: Michele Berselli*</sub>

---

<!-- This section relies on the html links generated by GitHub Pages 
and will not render correctly in Markdown -->
<div style="text-align: right">
    <a href="/pipelines-docs/"> Home </a> -
    <a href="0_Overview.html"> Overview </a> -
    <a href="1_Alignment.html"> Alignment </a> -
    <a href="2_Duplicate_Reads.html"> Duplicate Reads </a> -
    <a href="3_Transcript_Quantification.html"> Transcripts </a> -
    <a> <b> Genes </b> </a>
</div>