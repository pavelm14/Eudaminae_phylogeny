# Phylogenomic analysis: From raw data to time-calibrated tree
This is my notebook describing the process of building a phylogeny using target capture and published whole-genome resequencing data. I will use a number of software and packages written in _Python_, _Bash_, _Perl_. The MetaCentrum Czech national cluster provides the computational resources and infrastructure for all the analyses.


## The study group
We focus on the skipper butterfly subfamily Eudaminae (Lepidoptera: Hesperiidae). This is a cool group whose members show signatures of phenotypic convergences at phylogenetic scale. There are several species classified in distinct genera (e.g., _Cecropterus_, _Telegonus_) that resemble each other in wing color and shape. This also means that within each Eudaminae genus there is high morphological plasticity.

You can have a look at some nice pictures of mounted and alive butterflies, and caterpilars, at [Butterflies of America](https://www.butterfliesofamerica.com/L/Hesperiidae.htm).

## The data
The primary dataset comes from our sampling and target capture approach. The baits to capture come from Espeland et al. (2018), the so-called Anchored Hybrid Enrichment (AHE) BUTTERFLY 1.0 kit., which targeted 425 loci. For wet-lab protocols, visit this repository's [page](https://github.com/pavelm14/Eudaminae_phylogeny/blob/master/laboratory/pre-sequencing%20protocol.md)

As a complementary dataset, Emmanuel Toussaint shared their sequences of several Eudaminae taxa. Their data come from a target capture approach using the BUTTERFLY 1.0 kit, and another kit targeting a subset of the 425 loci, the so-called legacy butterfly gene markers.

The published genomes come from Li et al. (2019), which used a whole-genome resequencing approach in 250 representatives of the skipper butterflies (Hesperiidae).

### Subfamily phylogeny paper
For this study, the sampling size consists of one specimen per species. In total, there are 217 species: 188 species of Eudaminae and 29 outgroup taxa coming from the family Hesperiidae.

- Our own data processed in Gothenburg and sequenced by RapidGenomics consists of 97 species: 8 species as outgroup taxa and 89 Eudaminae species.

- Manu Toussaint data consists of 18 Eudaminae species: 8 species sequenced with the BUTTERFLY v2 kit (~350 loci) and 10 species with legacy, Sanger-era loci.

- The published genomes data consists of 100 species: 21 species as outgroup taxa and 79 Eudaminae species.

- Finally, I included 2 Eudaminae species with legacy, Sanger-era loci coming from Sahoo et al. Hesperiidae phylogeny.

## The MetaCentrum
The computational resources and infrastructure is facilitated by [MetaCentrum VO](https://metavo.metacentrum.cz/en/about/index.html), which is available to researchers based in the Czech Republic. My user account is [pavelmatos](https://metavo.metacentrum.cz/pbsmon2/user/pavelmatos).

### References
- Espeland et al. (2018) A comprehensive and dated phylogenomic analysis of butterflies. Current Biology 28(5): 770&ndash;778. DOI: [10.1016/j.cub.2018.01.061](https://doi.org/10.1016/j.cub.2018.01.061)

- Li et al. (2019) Genomes of skipper butterflies reveal extensive convergence of wing patterns. PNAS 116 (13) 6232&ndash;6237. DOI: [10.1073/pnas.1821304116](https://doi.org/10.1073/pnas.1821304116)
