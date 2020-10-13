# Using the Iso-Seq Application on SMRTlink and BioConda

Elizabeth Tseng, Principal Scientist, PacBio

## Why use Iso-Seq analysis?

ISO-SEQ ANALYSIS MAIN FEATURES
* No reference genome required
* No transcriptome assembly required
* Recovers full-length (5’ to 3’) transcripts - Yields highly accurate (>99%) transcripts

HIFI READS FROM CCS
![](./figures/bioconda1.PNG)

FULL-LENGTH READS HAVE 5’ AND 3’ PRIMERS
![](./figures/bioconda2.PNG)

REMOVE CONCATEMERS AND POLY(A) TAILS
![](./figures/bioconda3.PNG)

CLUSTER TO GET ISOFORMS
![](./figures/bioconda4.PNG)
* High Quality (HQ):
accuracy ≥99% and ≥2 FLNC read support
* Low Quality (LQ):
accuracy <99% and ≥2 FLNC read support

MAP AND COLLAPSE ISOFORMS
![](./figures/bioconda5.PNG)

BENEFITS OF ISO-SEQ ANALYSIS APPLICATION
* High-quality transcripts
* Full-Length Non-concatemer reads
* Mapped & collapsed isoforms
* Removes artifacts
* Removes poly(A) tails
