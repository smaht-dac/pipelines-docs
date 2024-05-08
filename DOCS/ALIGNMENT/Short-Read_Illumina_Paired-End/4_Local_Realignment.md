
# Local Realignment

In this step, the pipeline perform local realignment at indel positions. Unlike genome aligners that consider each read independently and may favor alignments with mismatches or soft-clips over opening a gap, local realignment takes into account all reads spanning a given position, enabling a high-scoring consensus that supports the presence of an indel event. The two-step realignment process identifies potential regions for alignment improvement and then realigns the reads in these regions using a consensus model that considers all reads in the alignment context together. This allows to correct potential mapping errors made by the aligners, enhancing the consistency of read alignments in regions with indels.

## Realigning Reads

##### Realign reads around indels

```text
sentieon driver -r reference.fasta
                -i deduped.bam
                --algo Realigner
                -k known_sites_INDEL.vcf
                realigned.bam
```

## Integrity Check

To confirm the integrity of the alignment BAM file, in-house Python code checks for the presence of the 28-byte empty block representing the EOF marker in BAM format.

## Implementation with Sentieon

The implementation uses Sentieon Realigner algorithm to identify the potential targets and perform local indel realignment. The pipeline is using Sentieon version 202308.01, corresponding to GATK versions 3.7, 3.8. The algorithm is equivalent to RealignerTargetCreator and IndelRealigner algorithms in GATK.

##### Identify potential targets (GATK equivalent)

```text
java -jar GenomeAnalysisTK.jar 
            -T RealignerTargetCreator
            -R reference.fasta
            -I deduped.bam 
            -known known_sites_INDEL.vcf
            -o realignment_targets.list
```

##### Local indel realignment (GATK equivalent)

```text
java -jar GenomeAnalysisTK.jar 
            -T IndelRealigner
            -R reference.fasta
            -I deduped.bam
            -targetIntervals realignment_targets.list
            -known known_sites_INDEL.vcf
            -o realigned.bam
```

## Source Code

All the relevant code can be accessed in the GitHub repository:

  - [sentieon_Realigner.sh](https://github.com/smaht-dac/sentieon-pipelines/blob/main/dockerfiles/sentieon/sentieon_Realigner.sh) [Realigner]