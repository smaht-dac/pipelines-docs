
# RSEM Reference

The RSEM Reference is generated from the standard Genome Reference Consortium Human Build 38 (GRCh38) released by the Broad Institute, as described in [GTEx](https://github.com/broadinstitute/gtex-pipeline) analysis pipeline.

The RSEM Reference uses GENCODE comprehensive gene annotations. For more detailed information please refer to the GENCODE documentation under “Genome Annotations” section.

## Downloading and Preparing the Genome Reference

##### Download the reference genome

```text
wget https://storage.googleapis.com/genomics-public-data/resources/broad/hg38/v0/Homo_sapiens_assembly38.fasta
```

##### ALT, HLA, and decoy contigs are excluded from the reference genome FASTA using the following Python code

```python
with open('Homo_sapiens_assembly38.fasta', 'r') as fasta:
    contigs = fasta.read()
contigs = contigs.split('>')
contig_ids = [i.split(' ', 1)[0] for i in contigs]

# exclude ALT, HLA and decoy contigs
filtered_fasta = '>'.join([c for i,c in zip(contig_ids, contigs)
    if not (i[-4:]=='_alt' or i[:3]=='HLA' or i[-6:]=='_decoy')])

with open('Homo_sapiens_assembly38_noALT_noHLA_noDecoy.fasta', 'w') as fasta:
    fasta.write(filtered_fasta)
```

##### Generate FASTA indexes

```text
samtools faidx Homo_sapiens_assembly38_noALT_noHLA_noDecoy.fasta

java -jar picard.jar \
    CreateSequenceDictionary \
    R=Homo_sapiens_assembly38_noALT_noHLA_noDecoy.fasta \
    O=Homo_sapiens_assembly38_noALT_noHLA_noDecoy.dict
```

## Generating RSEM Reference

##### Generate RSEM reference

```text
rsem-prepare-reference \
    --gtf gencode.annotation.gtf \
    Homo_sapiens_assembly38_noALT_noHLA_noDecoy.fasta \
    rsem_reference
```

## Implementation with RSEM

The current reference was generated using [RSEM](https://github.com/deweylab/RSEM) version v1.3.3.

---

<!-- This section relies on the html links generated by GitHub Pages 
and will not render correctly in Markdown -->
<div style="text-align: right">
    <a href="/pipelines-docs/"> Home </a> -
    <a href="0_BWT_Index.html"> BWT Index </a> -
    <a href="1_STAR_Index.html"> STAR Index </a> -
    <a> <b> RSEM Reference </b> </a>
</div>
