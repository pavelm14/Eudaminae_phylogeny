# Installing R packages - [ClockstaR2](https://github.com/sebastianduchene/ClockstaR)

We will work in the frontend `brno2` (skirit), We will use the R version 4.0.0-gcc available at MetaCentrum and note that all packages will be installed in a local directory.

Let's move to the working directory. Here, we need to have a concatenated phylogeny that will be used to test clock partitions. We already inferred the phylogeny using IQTREE2. Let's copy such a file into our working directory:

```
pavelmatos@skirit:~$

cd ~/overbeckia/clockstaR/input

cp ../../iqtree/total.partitions.treefile .

```

You'll also see that in one directory above (../partitionfinder), there are two file, 1) the concatenated fasta file of all the sequenced loci, and 2) the output file of PartitionFinder2. However, we need to slightly modify the latter file because ClockstaR has apparently been written with a slightly different format.

You need to find the line containing `RaxML-style partition definitions` and make sure that immediately under the line there is the first partition line. Otherwise modify accordingly:

```
nano ../partitionfinder/best_scheme_clockstar.txt

## Note the empty line after the last partition in RaxML-style
>>> .
>>> .
>>> .
>>> RaxML-style partition definitions
>>> DNA, Subset1 = 1192-1649\3, 834-1191\3, 1-673\3, 1775-2195\3
>>> DNA, Subset2 = 2-673\3
>>> DNA, Subset3 = 3-673\3, 835-1191\3, 833-1191\3
>>> DNA, Subset4 = 1650-1774, 674-832
>>> DNA, Subset5 = 1193-1649\3, 2197-2854\3
>>> DNA, Subset6 = 1194-1649\3, 2198-2854\3
>>> DNA, Subset7 = 1776-2195\3, 1777-2195\3
>>> DNA, Subset8 = 2196-2854\3
>>>
>>>
>>> MrBayes block for partition definitions
>>> .
>>> .
>>> .

```

Let's work now in R. First, you need to set a directory as default to install and load packages from. We already created the `R_LIB` in ~/.bashrc.

```bash
nano ~/.bashrc
## Add the following to the last line in .bashrc file, if not yet
>>> export R_LIBS="/storage/brno2/home/pavelmatos/software/R/packages"
```

All the libraries installed in the directory above will only work for `R-4.0.0-gcc`. You can of course install your own R version locally at your home directory, but note that you need to create another R packages folder.

```bash
source ~/.bashrc
module add R-4.0.0-gcc

## To open R console
R
```

Once in the R console, you will be able to install and load packages from a local directory. You do not need to specify `lib.loc` because you have your local directory for R packages in the $PATH.

```R
.libPaths() # This checks that you correctly called your local library.

## Let's install `devtools` to easily install packages from GitHub
install.packages("devtools")

## Remember that if you don't have `R_LIBS` specified in your $PATH, you can still install the package at your library location
# install.packages("devtools", lib="/storage/plzen1/home/pavelmatos/software/R/packages")

# Let's load `devtools`
library(devtools)
#Alternatively,
#library(devtools, lib.loc="/storage/brno2/home/pavelmatos/software/R/packages")

## To install packages from GitHub, we will use the devtools' function `install_github`
devtools::install_github("sebastianduchene/ClockstaR")

## Or
#devtools::install_github("sebastianduchene/ClockstaR", lib="/storage/brno2/home/pavelmatos/software/R/packages")

## Let's also install phytools and load all the packages needed to run `phybase`
library(ClockstaR2) # this package is already installed in MetaCentrum

```

Now, let's create the separate fasta alignments for each of the partitions found by PartitionFinder2

```
partition_data_partitionfinder("../partitionfinder/total_clockstar.fasta", "../partitionfinder/best_scheme_clockstar.txt")

```

Now, we will have 8 different .fasta files, each per partition, in the working directory, along with the concatenated ML phylogeny inferred in IQTREE2.

Let's now optimize the branch lengths of the 8 gene trees. Direct to the input folder where there are .fasta alignments per partition and the concatenated tree in newick format

```
optim.trees.interactive(folder.parts = "/storage/brno2/home/pavelmatos/overbeckia/clockstaR/input")

##If the R command above does not work, try installing phangorn and repeat
##install.packages("phangorn")

```

If the command above is working, you will be asked to name the file to store all the optimized gene tree. Just type `gene.trees`.

After that, ClockstaR2 will ask you whether to use separate substitution models for each partition, or use the JC for all. Since we already know the best partition scheme for each partition from PartitionFinder (search = beast), then we will type `y` and specify the model, gamma rate variation (4 categories) and proportion of invariant sites.

Finally, let's run ClockstaR2 to test different clock partitions. This step is in interactive mode:

```
clockstar.interactive()

##You will be asked to type the path to the GENE TREES in newick format:
/storage/brno2/home/pavelmatos/overbeckia/clockstaR/input/gene.trees

```

If you have previously installed the packages `foreach` and `doParallel`, you will be given the option to run the job in parallel. This can be good for large datasets.

After estimating the tree distances, ClockstaR2 will print the following message:

```
"I finished calculating the sBSDmin distances between trees"
The settings for clustering with ClockstaR are:
PAM clustering algorithm
K from 1 to number of data subsets-1
SEmax criterion to select the optimal k
500 bootstrap replicates
 Are these correct? (y/n)

```

These settings for the clustering algorithm is good for most datasets, so we will type `y` to the question. For changing these options, see more details in Kaufman and Rouseeuw (2009).

Once the analyses are finished, you will get the best number of clock partitions and you will be asked whether to sve the results in PDF:

```
[1] "ClockstaR has finished running"
[1] "The best number of partitions for your data set is: 3"
Do you wish to save the results in a pdf file?(y/n)

```

Type `y` and then type the name of the file (and path if you want to save the files in another directory that is not the working folder).

```
What should be the name and path of the output file?

```

Just type `clockstar_run`.

For more details on the interpretation of the graphs, visit the [program's Github page](https://github.com/sebastianduchene/ClockstaR) and the references below.

### References

#### Ancient gene flow detection
- Cai et al. (2020) The perfect storm: Gene tree estimation error, incomplete lineage sorting, and ancient gene flow explain the most recalcitrant ancient Angiosperm clade, Malpighiales. bioRxiv 2020.05.26.112318. DOI: [10.1101/2020.05.26.112318](https://doi.org/10.1101/2020.05.26.112318)

#### R package `phybase`
- http://faculty.franklin.uga.edu/lliu/phybase

#### R package `phytools`
- Revell (2012) phytools: an R package for phylogenetic comparative biology (and other things). Methods in Ecology and Evolution 3(2): 217-223. DOI: [10.1111/j.2041-210X.2011.00169.x](https://doi.org/10.1111/j.2041-210X.2011.00169.x)

#### R package `ape`
- Paradis et al. (2004) APE: Analyses of Phylogenetics and Evolution in R language. Bioinformatics 20(2): 289-290. DOI: [10.1093/bioinformatics/btg412](https://doi.org/10.1093/bioinformatics/btg412)
