# Using the Iso-Seq Application on SMRTlink and BioConda

Elizabeth Tseng, Principal Scientist, PacBio

## Why use Iso-Seq analysis?

**ISO-SEQ ANALYSIS MAIN FEATURES**
* No reference genome required
* No transcriptome assembly required
* Recovers full-length (5’ to 3’) transcripts
* Yields highly accurate (>99%) transcripts

**HIFI READS FROM CCS**

![](./figures/bioconda1.PNG)

**FULL-LENGTH READS HAVE 5’ AND 3’ PRIMERS**

![](./figures/bioconda2.PNG)

**REMOVE CONCATEMERS AND POLY(A) TAILS**

![](./figures/bioconda3.PNG)

**CLUSTER TO GET ISOFORMS**

![](./figures/bioconda4.PNG)

* High Quality (HQ):
accuracy ≥99% and ≥2 FLNC read support
* Low Quality (LQ):
accuracy <99% and ≥2 FLNC read support

**MAP AND COLLAPSE ISOFORMS**

![](./figures/bioconda5.PNG)

**BENEFITS OF ISO-SEQ ANALYSIS APPLICATION**
* High-quality transcripts
* Full-Length Non-concatemer reads
* Mapped & collapsed isoforms
* Removes artifacts
* Removes poly(A) tails

## Iso-Seq Analysis Using pbBioConda

**INSTRUCTIONS TUTORIAL**

Follow the instructions tutorial for installing all the software needed.
* If you do not have an HPC server to install pbbioconda, you should have already:
  - Create an AWS account
  - Create an AWS Linux Instance to run Iso-Seq 3 Analysis Pipeline
  - Connect to your AWS Instance
* Upgrades and Install Software

**DOWNLOAD THE DATA** [here](https://downloads.pacbcloud.com/public/dataset/ISMB_workshop/)

![](./figures/bioconda6.PNG)

Example:
```
$ wget –nv https://downloads.pacbcloud.com/public/dataset/ISMB_workshop/isoseq3/alz.ccs.bam
```

**SPECIFY ISO-SEQ PRIMERS**
```
$ more primers.fasta
```
<div class="output"> >5p
GCAATGAAGTCGCAGGGTTGGG
</div>
<div class="output"> >3p
GTACTCTGCGTTGATACCACTGCTT
</div>
![](./figures/bioconda7.PNG)

**INPUT CCS BAM FILE**
```
$ samtools view -h alz.ccs.bam
```
<div class="output">
m141008_060349_42194_c100704972550000001823137703241586_s1_p0/63/ccs4*0255
**00CCCGGGGATCCTCTAGAATGC~~~~~~~~~~~~~~~~~~~~~RG:Z:83ba013f np:i:35
rq:f:0.999682 sn:B:f,11.3175,6.64119,11.6261,14.5199 zm:i:63
</div>

**REFERENCE GENOME**
```
$ grep '>’ hg38.fa # to list the headers per chromosome
```
<div class="output">

>chr1 AC:CM000663.2 gi:568336023 LN:248956422 rl:Chromosome M5:6aef897c3d6ff0c78aff06ac189178dd AS:GRCh38

>chr2 AC:CM000664.2 gi:568336022 LN:242193529 rl:Chromosome M5:f98db672eb0993dcfdabafe2a882905c AS:GRCh38

>chr3 AC:CM000665.2 gi:568336021 LN:198295559 rl:Chromosome M5:76635a41ea913a405ded820447d067b0 AS:GRCh38

>chr4 AC:CM000666.2 gi:568336020 LN:190214555 rl:Chromosome M5:3210fecf1eb92d5489da4346b3fddc6e AS:GRCh38

>chr5 AC:CM000667.2 gi:568336019 LN:181538259 rl:Chromosome M5:a811b3dc9fe66af729dc0dddf7fa4f13 AS:GRCh38 hm:47309185-49591369
…

</div>


**SOFTWARE INSTALLATION CHECK**

Access to your conda environment
```
$ source activate <name of your environment>
```

Check your installation
```
$ isoseq3 --version
```
<div class="output">
isoseq3 3.4.x
</div>
```
$ lima --version
```
<div class="output">
lima 1.11.0
</div>
```
$ pbmm2 –-version
```
<div class="output">
pbmm2 1.3.0
</div>

## ISO-SEQ WORKFLOW

![](./figures/bioconda8.PNG)

![](./figures/bioconda9.PNG)
* Align to reference genome
* Remove redundancy

**PRIMER REMOVAL & DEMULTIPLEXING**

![](./figures/bioconda10.PNG)

Command line:
```
lima --isoseq --dump-clips --peek-guess -j 24\
```
<div class="output">
alz.ccs.bam isoseq_primers.fasta alz.demult.bam
</div>

Input files:
  alz.ccs.bam #HiFi reads
  isoseq_primers.fasta #Iso-Seq primers

Output files:
  alz.demult.bam

Options:
  --isoseq #specialized isoseq option for lima
  --dump-clips # show the clipped primers
  --peek-guess # remove spurious false positive signal
  -j 24 # Number of threads to use

After completion, you will see the following files:
```
$ ls -ltrh
```
![](./figures/bioconda11.PNG)

**TRIMMING POLY(A) TAILS AND CONCATEMER REMOVAL**

![](./figures/bioconda12.PNG)

Command line:
```
isoseq3 refine --require-polya\
```
<div class="output">
alz.demult.5p--3p.bam\ isoseq_primers.fasta
alz.flnc.bam
</div>

Input files:
  alz.demult.5p--3p.bam
  isoseq_primers.fasta

Output files:
  alz.flnc.bam

Options:
  --require-polya #if your transcripts have a polyA tail

After completion, you will see the following files:
```
$ ls -ltrh
```
![](./figures/bioconda13.PNG)

**ISOFORMS**

![](./figures/bioconda14.PNG)

Command line:
```
isoseq3 cluster alz.flnc.bam alz.polished.bam \
--verbose --use-qvs
```

Input files:
  alz.flnc.bam

Output files:
  alz.flnc.bam

Options:
  --verbose #if your transcripts have a polyA tail
  --use-qvs #Use CCS QVs, sets --poa-cov 100

After completion, you will see the following files:
```
$ ls -ltrh
```
![](./figures/bioconda15.PNG)
Note: Because the ccs input is Polished, the isoseq3 cluster output is already polished!

**MAP**

![](./figures/bioconda9.PNG)

Command line:
```
pbmm2 align hg38.fa alz.polished.hq.bam alz.aligned.bam
-j 24 --preset ISOSEQ –sort --log-level INFO
```

Input files:
  alz.polished.hq.bam
  hg38.fa

Output files:
  alz.aligned.bam

Options:
  -j 24 #Number of threads to use
  --preset ISOSEQ #select the alignment mode
  --sort #Generate sorted BAM file
  --log-level INFO #show progress

After completion, you will see the following files:
```
$ ls –ltrh
```
<div class="output">
alz.aligned.bam
alz.aligned.bam.bai
</div>

**COLLAPSE**

Command line:
```
isoseq3 collapse alz.aligned.bam alz.collapsed.gff
```

Input files:
  alz.aligned.bam

Output files:
  alz.collapsed.gff

After completion, you will see the following files:
```
$ ls –ltrh
```
![](./figures/bioconda16.PNG)

**PUBLICLY AVAILABLE ISO-SEQ DATA SETS** [here](https://github.com/PacificBiosciences/IsoSeq_SA3nUP/wiki/Iso-Seq-in-house-datasets)

![](./figures/bioconda17.PNG)

**ISO-SEQ ANALYSIS TERMINOLOGY**

![](./figures/bioconda18.PNG)


The pdf to this documentation can be found [here](https://raw.githubusercontent.com/ucdavis-bioinformatics-training/ucdavis-bioinformatics-training.presentations/master/isoseq/liz/2-Liz-Iso-Seq%20with%20SL%20and%20BioConda.pdf)
