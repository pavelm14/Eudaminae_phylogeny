# Working with NGS data

The target capture sequence raw data as it was sent by RapidGenomics are stored in: 1) **My laptop**, 2) **External HD**, 3) **Albiorix Gothenburg cluster (~/proj/data25)**, and 4) **MetaCentrum Czech national cluster (/storage/brno3-cerit/home/pavelmatos/eudaminae/raw)**.

## The MetaCentrum infrastructure
There are several frontends, or machines, that can be used directly via log in without reservation. I've been regularly using 4 frontends regularly:
- `skirit.ics.muni.cz` (home directory in **_brno2_**) for small analyses such as diversification and biogeography,
- `zuphux.cerit-sc.cz` (home directory in **_brno3-cerit_**) for high memory jobs, up to a few TB of memory,
- `nympha.zcu.cz` (home directory in **_plzen1_**) for running NGS-related analyses because it has 3 TB space quota for storing, and
- `storage-brno3-cerit.metacentrum.cz` (home directory in **_brno3-cerit_**) for storing raw NGS data because it doesn't have a space quota.

Have a look at the MetaCentrum Wiki pages for further basic information about the [frontends](https://wiki.metacentrum.cz/wiki/Frontend), [data transfer](https://wiki.metacentrum.cz/wiki/Working_with_data), or how to [submit jobs](https://wiki.metacentrum.cz/wiki/How_to_compute/Batch_jobs).

### Log in to MetaCentrum
I use the home portable version of [MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html) to create an environment in MetaCentrum from my Windows laptop. I use `ssh` to log in into any MetaCentrum frontend, for example, below I log in to the _nympha_ frontend in Plzen where I run all NGS-related analyses.

```bash
ssh -x pavelmatos@nympha.metacentrum.cz #To run NGS data analyses
ssh -x pavelmatos@zuphux.cerit-sc.cz #To run high-memory demanding jobs
ssh -x pavelmatos@skirit.metacentrum.cz #To run diversification and biogeography analyses
ssh -x pavelmatos@storage-brno3-cerit.metacentrum.cz #To back up data. This is the same as @zuphux.cerit-sc.cz but can't run any job here
```
## Software installation in MetaCentrum
MetaCentrum has a [page](https://wiki.metacentrum.cz/wiki/How_to_install_an_application) detailing how to install software, packages written in several programming languages, or using GUI to install some applications.

I also describe in separate entries how to install the following relevant software in MetaCentrum:

- **SECAPR, SEquence CApture PRocessor** ([Andermann et al. 2018](https://doi.org/10.7717/peerj.5175)): This is a pipeline for processing targeted enriched Illumina sequences, from raw data to alignments. However, I also use this pipeline to process whole-genome resequencing data.

- **SRA toolkit** ([NCBI 2011&ndash;](https://www.ncbi.nlm.nih.gov/books/NBK158900/)): This is collection of packages for downloading and managing SRA data from GenBank. I use this for getting the whole-genome resequencing data from [Li et al. (2019)](https://www.pnas.org/content/116/13/6232).

## Retrieve data from local disk to cluster (and vice versa)
I use MobaXterm to transfer data between my local computer and the MetaCentrum. For example, I want to back up a copy of the raw target capture sequencing data:

```bash
cd /drives/d/PAVEL/Lepidoptera\ projects/Hesperiidae/Laboratory/RapidGenomics/data/raw/
```

There are 290 files with compressed sequences (.tar.gz) which correponds to the R1 and R2 reads for 145 skipper specimens.

To copy these recursiverly to my back up folder in `storage-brno3-cerit.metacentrum.cz`

```bash
scp -r ./*.fastq.gz pavelmatos@zuphux.cerit-sc.cz:/home/pavelmatos/eudaminae/raw
```

If many large directories are needed, a batch download of part of them might be better. In this case, I upload the first 30 specimens having clean (trimmed) reads. The sequences are stored uncompressed (.fastq) for each specimen in ony numbered folder (from 001/ to 145/).

```bash
for i in {001..030}; 
do scp -r ../../../NGS\ bioinformatics/processed/2_cleaned_trimmed_reads/"$i"_clean/ pavelmatos@nympha.metacentrum.cz:/home/pavelmatos/eudaminae/processed/2_cleaned_trimmed_reads;
done
```

After uploading files from my own computer to the cluster, the user/group at the cluster will not have rights to write nor execute the data. Therefore, I might need to change permissions in some cases.

To change modes to `user`, `group`, `others` to `write`, `read`, `execute` on a bunch of folders or files, the easiest way is to run the following example for directories:

```bash
find ./folder/ -type d -exec chmod 755 {} \;
```

And this is for files within a folder:

```bash
find ./folder/ -type f -exec chmod 755 {} \;
```

The modes can be changed using numbers. For example, `chmod 755` means full access (7: rwx) to the `user`, read and execute modes (5: r-x) to both the `group` and `others`. Another example, `chmod 644` means read and write access (6: rw-) to the `user`, and read-only access (4: r--) to both the `group` and `others`. You can [have a look](https://en.wikipedia.org/wiki/Chmod) to what the numbers, from 0 to 7, mean when using `chmod`.

If you want to check the size of a directory, use the `du` (disk usage) command with the options `-s` for summary and `-h` for human readable mode

```bash
du -sh /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
```

