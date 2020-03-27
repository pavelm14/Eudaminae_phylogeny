# Working with NGS data

The target capture sequence raw data as it was sent by RapidGenomics are stored in: 1) **My laptop**, 2) **External HD**, 3) **Albiorix Gothenburg cluster (~/proj/data25)**, and 4) **MetaCentrum Czech national cluster (_NOT YET!_)**.

## The MetaCentrum infrastructure
There are several frontends, or machines, that can be used directly via log in without reservation. I've been using 4 frontends regularly, `skirit.ics.muni.cz` (home directory in **_brno2_**) for small analyses such as diversification and biogeography, `zuphux.cerit-sc.cz` (home directory in **_brno3-cerit_**) for high memory jobs, up to a few TB of memory, `nympha.zcu.cz` (home directory in **_plzen1_**) for running NGS-related analyses because it has 3 TB space quota for storing, and `storage-brno3-cerit.metacentrum.cz` (home directory in **_brno3-cerit_**) for storing raw NGS data because it doesn't have a space quota.

Have a look at the MetaCentrum Wiki pages for further basic information about the [frontends](https://wiki.metacentrum.cz/wiki/Frontend), [data transfer](https://wiki.metacentrum.cz/wiki/Working_with_data), or how to [submit jobs](https://wiki.metacentrum.cz/wiki/How_to_compute/Batch_jobs).

### Log in to MetaCentrum
I use the home portable version of [MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html) to create an environment in MetaCentrum from my Windows laptop. 
