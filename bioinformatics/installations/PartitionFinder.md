# PartitionFinder2

We use this program to find the best-fit partitioning strategy for phylogenetic inference.

The documentation is available [here](http://www.robertlanfear.com/partitionfinder/). By the time I installed PartitionFinder2, the version was 2.1.1. It is the last version since December 2016.

It is also possible nowadays to run PartitionFinder2 directly from IQTREE 2, though the implementation might be different and may give not exactly the same but highly similar results.

```bash
pavelmatos@nympha:~$
cd software/
git clone https://github.com/smirarab/ASTRAL.git
cd Astral/
module add jdk-8 #I need to add java to my environment for running the .sh file below. Java is already in Metacentrum as jdk-8
./make.sh
```

This is it! ASTRAL-III is installed at `/storage/plzen1/home/pavelmatos/software/ASTRAL/astral.5.7.3.jar`.

## ASTRAL in frontend brno2!!!
ASTRAL 5.6.3 is also installed at `pavelmatos@skirit:~$`. Unfortunately, on October 21, 2019, there was a shutdown on the server room `brno2` and by now everything in the path has changed. This has screwed all my installations, such as R packages, etc.

Fortunately, ASTRAL is still installed correctly and I can call the program using the old path `/storage/brno2/home/pavelmatos/software/ASTRAL/astral.5.6.3.jar`.

### References
- Zhang et al. (2018) ASTRAL-III: Polynomial Time Species Tree Reconstruction from Partially Resolved Gene Trees. BMC Bioinformatics 19(S6): 153. DOI: [10.1186/s12859-018-2129-y](https://doi.org/10.1186/s12859-018-2129-y)
