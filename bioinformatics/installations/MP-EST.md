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
>>> ARCHITECTURE ?= unix 
## Ctrl+x -> Save Y

## Install MP-EST2.0
make
```

To add MP-EST 2.0 to the $PATH environment

```bash
pavelmatos@nympha:~/software/mp-est/src$

nano ~/.bashrc # nano /storage/plzen1/home/pavelmatos/.bashrc
>>> export PATH="/storage/plzen1/home/pavelmatos/software/mp-est/src:$PATH"
## Ctrl+x -> Save Y

## Run MP-EST 2.0
source ~/.bashrc
mpest
```

### References
- Liu et al. (2010) A maximum pseudo-likelihood approach for estimating species trees under the coalescent model. BMC Evolutionary Biology 10: 302. DOI: [10.1186/1471-2148-10-302](https://doi.org/10.1186/1471-2148-10-302)
