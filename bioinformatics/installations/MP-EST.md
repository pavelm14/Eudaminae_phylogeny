# MP-EST

We use this program to estimate species trees based on rooted, binary gene trees. The algorithm used by this program resembles the multispecies coalescent in that the species phylogeny is informed by the different gene tree topologies, thus, handling potential incomplete lineage sorting (ILS). However, this bear in mind that this is not a strict multispecies coalescent model because the gene and species trees topologies are not jointly estimated.

MP-EST infer species trees with branches in coalescent units of rooted triples. The documentation is available [here](http://faculty.franklin.uga.edu/lliu/mp-est). By the time I installed MP-EST, the latest stable version was 2.0. The manual is [here](https://faculty.franklin.uga.edu/lliu/sites/faculty.franklin.uga.edu.lliu/files/manual_mpest2.0.pdf) and the GitHub repository is [here](https://github.com/lliu1871/mp-est).

```bash
pavelmatos@nympha:~$
cd software/

git clone git clone https://github.com/lliu1871/mp-est.git
cd mp-est/src

## You need to specify the OS in the makefile
nano makefile
>>>ARCHITECTURE ?= unix 
## Ctrl+x -> Save Y
```

This is it! ASTRAL-III is installed at `/storage/plzen1/home/pavelmatos/software/ASTRAL/astral.5.7.3.jar`.

## ASTRAL in frontend brno2!!!
ASTRAL 5.6.3 is also installed at `pavelmatos@skirit:~$`. Unfortunately, on October 21, 2019, there was a shutdown on the server room `brno2` and by now everything in the path has changed. This has screwed all my installations, such as R packages, etc.

Fortunately, ASTRAL is still installed correctly and I can call the program using the old path `/storage/brno2/home/pavelmatos/software/ASTRAL/astral.5.6.3.jar`.

### References
- Zhang et al. (2018) ASTRAL-III: Polynomial Time Species Tree Reconstruction from Partially Resolved Gene Trees. BMC Bioinformatics 19(S6): 153. DOI: [10.1186/s12859-018-2129-y](https://doi.org/10.1186/s12859-018-2129-y)
