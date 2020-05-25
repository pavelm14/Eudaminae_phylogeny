# Creating a new dataset

Once we have our clean alignments, the next step is to create datasets for different type of analyses. The main goals here are: 1) to rename specimen names in every alignment file, 2) to subset the data, 3) to count the number of loci per specimen or the number of specimens per locus we have in our datasets, and 4) to concatenate and add missing sequences to specimens in every fasta file.

## Software that we need
We will need to have three [pipelines already installed](https://github.com/pavelm14/Eudaminae_phylogeny/tree/master/bioinformatics/installations) for this part:
- SeqKit: a cross-platform toolkit for phylogenomic fasta manipulation. This will allow us to batch rename taxa in every fasta file.
- SECAPR: a semi-automated pipeline that will allow us to add missing sequences to specimens in every fasta file.
- _catfasta2phyml_: a program written in perl that will allow us to concatenate and get partition files.
