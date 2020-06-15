# Installing R packages

Several R packages desigined for evolutionary study are already installed in MetaCentrum. However, it is very likely that you want to use packages that are not there, so you need to install them locally in your home directory.

First, you need to set a directory as default to install and load packages from. To do this, we create and `R_LIB`.

```bash
pavelmatos@nympha:~$
mkdir -p software/R/packages

nano ~/.bashrc
## Add the following to the last line in .bashrc file
>>> export R_LIBS="/storage/plzen1/home/pavelmatos/software/R/packages"
```

By the time writing this tutorial, the latest R version installed in MetaCentrum was `R-4.0.0-gcc`. You can of course install your own R version locally at your home directory.

```bash
pavelmatos@nympha:~$
source ~/.bashrc
module add R-4.0.0-gcc ## If you want to use the latest available R version in MetaCentrum, use `module add R`

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

## Again, if you don't have `R_LIBS` in your $PATH, you can use the following, provided that you installed the package there
# library(devtools, lib.loc="/storage/plzen1/home/pavelmatos/software/R/packages")

## To install packages from GitHub, we will use the devtools' function `install_github`
devtools::install_github("lliu1871/phybase")

## Or
devtools::install_github("lliu1871/phybase", lib="/storage/plzen1/home/pavelmatos/software/R/packages")

## Let's also install phytools and load all the packages needed to run `phybase`
devtools::install_github("liamrevell/phytools", lib="/storage/plzen1/home/pavelmatos/software/R/packages")
library(phytools, lib.loc="/storage/plzen1/home/pavelmatos/software/R/packages")
library(phybase, lib.loc="/storage/plzen1/home/pavelmatos/software/R/packages")
library(ape) # this package is already installed in MetaCentrum

## Quit R
q()
```

### References

#### Ancient gene flow detection
- Cai et al. (2020) The perfect storm: Gene tree estimation error, incomplete lineage sorting, and ancient gene flow explain the most recalcitrant ancient Angiosperm clade, Malpighiales. bioRxiv 2020.05.26.112318. DOI: [10.1101/2020.05.26.112318](https://doi.org/10.1101/2020.05.26.112318)

#### R package `phybase`
- http://faculty.franklin.uga.edu/lliu/phybase

#### R package `phytools`
- Revell (2012) phytools: an R package for phylogenetic comparative biology (and other things). Methods in Ecology and Evolution 3(2): 217-223. DOI: [10.1111/j.2041-210X.2011.00169.x](https://doi.org/10.1111/j.2041-210X.2011.00169.x)

#### R package `ape`
- Paradis et al. (2004) APE: Analyses of Phylogenetics and Evolution in R language. Bioinformatics 20(2): 289-290. DOI: [10.1093/bioinformatics/btg412](https://doi.org/10.1093/bioinformatics/btg412)
