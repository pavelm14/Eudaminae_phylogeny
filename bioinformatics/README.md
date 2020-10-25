# Working with NGS data

The target capture sequence raw data as it was sent by RapidGenomics are stored in: 1) **My laptop**, 2) **External HD**, 3) **Albiorix Gothenburg cluster (~/proj/data25)**, and 4) **MetaCentrum Czech national cluster (/storage/brno3-cerit/home/pavelmatos/eudaminae/raw)**.

## 1. The MetaCentrum infrastructure
There are several frontends, or machines, that can be used directly via log in without reservation. I've been regularly using 4 frontends:
- `skirit.ics.muni.cz` (home directory in **_brno2_**) for small analyses such as diversification and biogeography,
- `zuphux.cerit-sc.cz` (home directory in **_brno3-cerit_**) for high memory jobs, up to a few TB of memory,
- `nympha.zcu.cz` (home directory in **_plzen1_**) for running NGS-related analyses because it has 3 TB space quota for storing, and
- `storage-brno3-cerit.metacentrum.cz` (home directory in **_brno3-cerit_**) for storing raw NGS data because it doesn't have a space quota.

Have a look at the MetaCentrum Wiki pages for further basic information about the [frontends](https://wiki.metacentrum.cz/wiki/Frontend), [data transfer](https://wiki.metacentrum.cz/wiki/Working_with_data), or how to [submit jobs](https://wiki.metacentrum.cz/wiki/How_to_compute/Batch_jobs).

### Log in to MetaCentrum
I use the home portable version of [MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html) to create an environment in MetaCentrum from my Windows laptop. I use `ssh` to log in into any MetaCentrum frontend, for example, below I log in to the _nympha_ frontend in Plzen where I run all NGS-related analyses.

```bash
[pavel local computer]$

ssh -x pavelmatos@nympha.metacentrum.cz ##To run NGS data analyses
ssh -x pavelmatos@zuphux.cerit-sc.cz ##To run high-memory demanding jobs
ssh -x pavelmatos@skirit.metacentrum.cz ##To run diversification and biogeography analyses
ssh -x pavelmatos@storage-brno3-cerit.metacentrum.cz ##To back up data. This is the same as @zuphux.cerit-sc.cz but can't run any job here
```

## 2. Software installation in MetaCentrum, from raw reads to alignments
MetaCentrum has a [page](https://wiki.metacentrum.cz/wiki/How_to_install_an_application) detailing how to install software, packages written in several programming languages, or using GUI to install some applications.

I also describe in separate entries how to install the following relevant software in MetaCentrum:

- **[SECAPR](https://github.com/pavelm14/Eudaminae_phylogeny/blob/master/bioinformatics/installations/SECAPR.md), SEquence CApture PRocessor** ([Andermann et al. 2018](https://doi.org/10.7717/peerj.5175)): This is a pipeline for processing targeted enriched Illumina sequences, from raw data to alignments. However, I also use this pipeline to process whole-genome resequencing data.

- **SRA toolkit** ([NCBI 2011&ndash;](https://www.ncbi.nlm.nih.gov/books/NBK158900/)): This is a collection of packages for downloading and managing SRA data from GenBank. I use this for getting the whole-genome resequencing data from [Li et al. (2019)](https://www.pnas.org/content/116/13/6232).

- For more software, have a look at the ["Installations" page](https://github.com/pavelm14/Eudaminae_phylogeny/tree/master/bioinformatics/installations).

## 3. Pipeline, from raw reads to alignments
This workflow contains:

- Retrieve data: The target-capture data are stored in my local computer while the WGS data are stored in GenBank. In this step, we retrieve all data to the cluster frontend.



