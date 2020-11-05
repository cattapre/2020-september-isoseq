# TAMA (hands-on)

Richard Kuo, Roslin Institute, University of Edinburgh

Remember to run these commands to obtain the data and files for TAMA
```
cd /share/workshop/isoseq_workshop/$USER/
cp /share/workshop/isoseq_workshop/rkuo/folder_copy.tar.gz .
tar -xzvf folder_copy.tar.gz
git clone https://github.com/GenomeRIK/tama.git
```

Load modules
```
module load samtools/1.10 bedtools2/2.29.2 biopython/1.71 bamtools/2.4.2 minimap2/2.17
```

Click [here](https://github.com/PacificBiosciences/IsoSeq/blob/master/isoseq-clustering.md) for the rest of the TAMA Tutorial we'll be following today. Start at the beginning where the header is labeled 'UC Davis TAMA Tutorial 1.'

Now, we'll move onto 'UC Davis TAMA Tutorial 2.'

Copy and paste these files into your browser, and it should prompt you to download them onto your computer so we can view them on IGV
```
ftp://ftp.ed.ac.uk/edupload/map_cds_tc_nc_nolde_mm2_alz_flnc_hg38.bed
ftp://ftp.ed.ac.uk/edupload/tm_tc_flnc_nolde_gencode_100.bed
```

We'll be viewing them in the IGV web app [here](https://igv.org/app/). Make sure it's loaded in hg38 like earlier. Then, load the two ftp files into IGV by clicking 'Tracks,' then 'Local File.'

You can also download IGV locally [here](http://software.broadinstitute.org/software/igv/)

Click [here](https://github.com/GenomeRIK/tama/wiki) for the TAMA wiki GitHub repository for more information and directions!
