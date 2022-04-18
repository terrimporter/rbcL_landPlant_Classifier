# Land plant rbcL reference set for the RDP Classifier

[![DOI](https://zenodo.org/badge/478168998.svg)](https://zenodo.org/badge/latestdoi/478168998)

This repository contains training sets that can be used with the Ribosomal Database Project classifier (Wang et al., 2007) to taxonomically assign land plant rbcL cpDNA sequences.  The latest release can be downloaded from https://github.com/terrimporter/rbcL_landPlant_Classifier/releases .  The trained files ready to be used with the RDP Classifier are available as well as the original files used for training (a taxonomy file and a FASTA file) are available as 'version-ref'.  We also provide a fasta file that can be used with locally-installed blastn when looking for similar species in the database.  A diatom-specific rbcL classifier is available at https://github.com/terrimporter/rbcLdiatomClassifier and a more general eukaryote rbcL classifier (includes land and non-land plants) is available at https://github.com/terrimporter/rbcLClassifier .

# How to cite

You can cite this repository directly:  
Teresita M. Porter. (2022) Land plant rbcL reference set for the RDP Classifier (Version 1.0.0). Zenodo. https://doi.org/10.5281/zenodo.6415246

You should also cite the Ribosomal Database Classifier:  
Wang et al. (2007) Naïve Bayesian classifier for rapid assignment of rRNA sequences into the new bacterial taxonomy.  Applied and Environmental Microbiology, 73: 5261.

# Quick Start
```linux
############ Install the RDP classifier if you need it
# The easiest way to install the RDP classifier v2.13 is using conda
conda install -c bioconda rdp_classifier
# Alternatively, you can install from SourceForge and run with java if you don't use conda
wget https://sourceforge.net/projects/rdp-classifier/files/rdp-classifier/rdp_classifier_2.13.zip
# decompress it
unzip rdp_classifier_2.13
# record path to classifier.jar ex. /path/to/rdp_classifier_2.13/dist/classifier.jar

############ Get the latest land plant rbcL training set
wget https://github.com/terrimporter/rbcL_landPlant_Classifier/releases/download/v1/rbcL_landPlant_v1.zip

# decompress it
unzip rbcL_landPlant_v1.zip

# record the path to the rRNAClassifier.properties file ex. /path/to/mydata_trained/rRNAClassifier.properties

############ Run the RDP Classifier 
# If it was installed using conda, run it like this:
rdp_classifier -Xmx8g classify -t /path/to/mydata_trained/rRNAClassifier.properties -o rdp.output query.fasta
# Otherwise run it using java like this:
java -Xmx8g -jar /path/to/rdp_classifier_2.13/classifier.jar -t /path/to/mydata_trained/rRNAClassifier.properties -o rdp.output query.fasta
```

# Releases

### v1

The rbcL_landPlant_v1_trained.zip file should be decompressed and used directly with the RDP Classifier to make taxonomic assignments.  

RbcL land plant (Embryophyta) records were mined from the NCBI nucleotide database [April 1/22]. Sequences were filtered to retain only those that are 500 bp+, contain no non-nucleotide characters, and have Linnean binomial species names.  Sequences were screened for bacterial contaminants. Bacterial and non-land plant outgroup sequences were added.

Taxonomic assignment results should be filtered according to their bootstrap support values to reduce false positive assignments. Cutoffs are based on leave-one-sequence-out testing of non-singleton genera. Here we recommend MINIMUM bootstrap cutoffs according to query length and assignment rank. Assuming your query sequences are represented in the reference set, using the cutoffs presented in the table below should ensure 80% accuracy. 

Bootstrap support cutoffs, 80% accuracy:  

Rank | 500 bp+ | 400 bp | 300 bp | 200 bp | 100 bp  
--- |:---:|:---:|:---:|:---:|:---:  
Superkingdom | 0 | 0 | 0 | 0 | 0  
Kingdom | 0 | 0 | 0 | 0 | 0  
Phylum | 0 | 0 | 0 | 0 | 0   
Class | 0 | 0 | 0 | 0 | 0  
Order | 0 | 0 | 0 | 0 | 0  
Family | 0 | 0 | 0 | 0 | 0  
Genus | 0 | 0 | 0 | 40 | 90   
Species | - | NA | NA | NA | NA  

NA = No cutoff available will result in 80% correct assignments 

* Leave one sequence out testing for query lengths 500 bp+ is in progress and will be updated when complete.

Generally speaking, for a 200bp query sequence, it is not possible to get species level assignments with at least 80% accuracy. Genus level assignments have at least 80% accuracy when using a 0.40 bootstrap support cutoff.  Without using any cutoffs (or a bootstrap cutoff of 0), then we would expect assignments to the family rank to be ~ 95% correct, and assignments to the order-kingdom ranks to be ~ 99% correct.

### v1-ref

The rbcL_landPlant_v1-ref_training.zip file should be decompressed.  The folder contains the original taxonomy and fasta files that are included here for reference only.  Sequences were mined from the NCBI nucleotide database in April 2022 and only includes sequences identified to the species rank that are at least 500 bp long.  The sequences have been screened to remove any potential bacterial contaminant sequences.  Taxonomic composition is largely comprised of land plants (Embryophyta) with some bacterial and non-land plant outgroup taxa.  

### v1-blastn

The rbcL_landPlant_v1-blastn.zip file should be unzipped.  The folder contains a FASTA formatted file that can be used to build a local database with makeblastdb and can be run using blastn.  This may be useful for finding similar species in the reference set.

```linux
# assuming you already have local blast intalled
# put the fasta file in your ncbi-blast-2.9.0+/db directory
# create a blast database using the fasta file
makeblastdb -in rbcL_landPlant.fasta -dbtype nucl -input_type fasta

# example, run blast keeping top hit only, tabular output, with 10 threads
blastn -query testquery.fasta -task megablast -db rbcL_landPlant.fasta -out test.blastn -evalue '1e-20' -outfmt 6 -max_target_seqs 1 -num_threads 10

```

# Additional information

For additional information on how to run the RDP classifier, see the RDP Classifier 2.13 help or the RDP Classifier v2.12 README.

The RDP classifier v2.13 can be downloaded from:
https://sourceforge.net/projects/rdp-classifier/

# References

Wang, Q., Garrity, G. M., Tiedje, J. M., & Cole, J. R. (2007). Naive Bayesian Classifier for Rapid Assignment of rRNA Sequences into the New Bacterial Taxonomy. Applied and Environmental Microbiology, 73(16), 5261–5267. Available from https://sourceforge.net/projects/rdp-classifier/

Last updated: April 18, 2022
