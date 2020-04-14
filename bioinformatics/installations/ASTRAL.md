# ASTRAL-III (Accurate Species TRee ALgorithm)

We use this program to estimate unrooted species trees based on unrooted gene trees. The algorithm used by this program resembles the multispecies coalescent in that the species phylogeny is informed by the different gene tree topologies, thus, handling potential incomplete lineage sorting (ILS). However, this bear in mind that this is not a strict multispecies coalescent model because the gene and species trees topologies are not jointly estimated.

The documentation is available [here](https://github.com/smirarab/ASTRAL). By the time I installed ASTRAL-III (5.6.3), it follows the algorithm described by [Zhang et al. (2018)](https://doi.org/10.1186/s12859-018-2129-y). You will always install the latest version of ASTRAL by following the code below.

```bash
pavelmatos@skirit:~$
cd software/
git clone https://github.com/smirarab/ASTRAL.git
cd Astral/
module add jdk-8 #I need to add java to my environment for running the .sh file below. Java is already in Metacentrum as jdk-8
./make.sh
```

This is it! ASTRAL-III is installed at `/storage/brno2/home/pavelmatos/software/ASTRAL/astral.5.6.3.jar`.

#### UPDATE!!!

Unfortunately, on October 21, 2019, there was a shutdown on the server room `brno2` and by now everything in the path has changed. This has screwed all my installations, such as R packages, etc.

Fortunately, ASTRAL is still installed correctly and I can call the program using the old path `/storage/brno2/home/pavelmatos/`.

#### References
