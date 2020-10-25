# Download SRA and extract fastq files

Next-gen sequence data are usually deposited at NCBI SRA (Sequence Read Archive). The main goals here are: 1) to download in batch genomic data, and 2) to manipulate SRA data (e.g., convert SRA to fastq format).

##### Table of Contents
- [Software](#software-that-we-need)  
1. [Batch download SRA](#1-batch-download-ncbi-sra-data)
2. [SRA to FastQ](#2-convert-sra-to-fastq)
3. [Jointly download and convert FastQ](#3-alternative-option-to-jointly-download-and-convert-sra-to-fastq)

## Software that we need
- [SRA Toolkit](https://github.com/pavelm14/Eudaminae_phylogeny/blob/master/bioinformatics/installations/SRAtoolkit.md): a toolkit for manipulating, downloading, and submitting genomic data.

## 1. Batch download NCBI SRA data
For this exercise, we will download the low-coverage whole genome re-sequencing (WGS) data from [Li et al. (2019)](https://www.pnas.org/content/116/13/6232). The NCBI Study Project associated to this data is [SRP147939](https://trace.ncbi.nlm.nih.gov/Traces/study/?acc=SRP147939). Follow the link and you will get the information for 248 WGS of skipper butterflies: in total 439.26 Gb and 983.14 Gbases.

At the NCBI Study Project, you will have the option to download data information, either with complete metadata or just the SRR Accession Numbers for the 248 specimens. The metadata table (SraRunTable.txt) contains information of the specimen (collection, etc) and the sequencing approach. The SRR Accession list (SRR_Acc_List.txt) contains the SRR numbers for all 248 specimens, one SRR Accession per line.

We will use the SRR_Acc_List.txt to download in batch the WGS data. The SRA toolkit's command `prefetch` will do the job. Because the files are quite large, we will set the max-size to 1000000000.

The job will take a lot of time given the amount of data to download. There are two options to run this job in Metacentrum: 1) use a `screen` that will work behind but with limited memory and cores, or 2) submit a proper job to the cluster, with more memory and cores option. For the sake of showing how both options work, I will use a `screen` to download SRA data and a proper submission of job in the next step to convert SRR to fastq.

```bash
pavelmatos@nympha:~$
cd eudaminae/data/janzen19 ## the SRR_Acc_List.txt is in this directory
source ~/.bashrc ## Activate the $PATH to use SRA Toolkit commands

## The basic use to download one WGS is
## prefetch --max-size 1000000000 SRR7174336

## If SRA Toolkit is not in the $PATH
## /home/pavelmatos/sratoolkit.2.9.6-1-ubuntu64/bin/prefetch --max-size 1000000000 SRR7174336

## Create a bash script to download all 248 WGS
while read line; 
do echo /home/pavelmatos/sratoolkit.2.9.6-1-ubuntu64/bin/prefetch --max-size 1000000000 $line;
done < SRR_Acc_List.txt > download.sh
```

The `while` loop above is reading the file SRR_Acc_List.txt and storing every line (every SRR accession) in the argument $line. Then, it's printing the path to the `prefetch` command and the arguments. The 248 lines of commands is finally stored in the file download.sh.

```bash
pavelmatos@nympha:~/eudaminae/data/janzen19$
screen -S download ## This creates a new screen with the name download where we will run behind our bash script

sh download.sh
```

Some options with the screen:
- To safely exit the screen , press `Ctrl+a` followed by `d`. Be very careful, because `Ctrl+d` kills the screen!
- Once you have detached, you can check your active screens using `screen -ls`.
- To re-attach to your screen, `screen -r download`.
- To kill a de-attached screen, `screen -X -S download quit`

The script download.sh will run for 3-4 days to download all the 439 Gb of data! Note that if for any reason the bash script is stoped, you can resume the download by calling again your script `sh download.sh`. You can check time to time the status by re-attaching to your screen. Once the download is complete, all the files will be saved at the directory `~/ncbi/public/sra`.

Go to [Table of Contents](#table-of-contents)

## 2. Convert SRA to fastq

For this exercise, we will now submit a proper job to the Metacentrum cluster. Converting the files into fastq format is memory consuming, so it makes sense to run the job in the Metacentrum's $SCRATCH directory. At the same time, we can be deleting the original SRA files to save space at our disk. We will only use and need the fastq files.

```bash
pavelmatos@nympha:~/eudaminae/data/janzen19$

while read line; 
do echo "fasterq-dump /storage/plzen1/home/pavelmatos/ncbi/public/sra/$line.sra -O /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
rm /storage/plzen1/home/pavelmatos/ncbi/public/sra/$line.sra";
done < SRR_Acc_List.txt > Metacentrum_SRA_to_FASTQ.sh

## Modify the beginning of the .sh file to have the complete instructions for the job
nano Metacentrum_SRA_to_FASTQ.sh

>>> #PBS -N sra2fastq_qsub
>>> #PBS -l select=1:ncpus=6:mem=200gb:scratch_local=100gb
>>> #PBS -l walltime=23:59:00
>>> 
>>> #clean scratch after the end
>>> trap 'clean_scratch' TERM EXIT
>>> 
>>> # go to scratch directory
>>> cd $SCRATCHDIR || exit 1
>>> 
>>> source /storage/plzen1/home/pavelmatos/.bashrc
>>> 
>>> fasterq-dump /storage/plzen1/home/pavelmatos/ncbi/public/sra/SRR7174339.sra -O /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
>>> rm /storage/plzen1/home/pavelmatos/ncbi/public/sra/SRR7174339.sra
>>> fasterq-dump /storage/plzen1/home/pavelmatos/ncbi/public/sra/SRR7174340.sra -O /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
>>> rm /storage/plzen1/home/pavelmatos/ncbi/public/sra/SRR7174340.sra
>>> etc ...

## Ctrl+x -> Save Y ## Save the changes and exit the nano text editor
```

To submit the job, just type `qsub Metacentrum_SRA_to_FASTQ.sh`. Check your job status in this website: https://metavo.metacentrum.cz/en/myaccount/myjobs.html

To speed up the process, you can create several bash scripts with less commands. In the above case, we were trying to convert 248 SRA files in one single job!

When the job is done, you can move the unmated reads to a different folder (e.g., /unmated_reads). We won't use them, we will work only with the paired _1.fastq and _2.fastq files.

```bash
pavelmatos@nympha:~/eudaminae/data/janzen19$
cd fastq
mkdir unmated_reads

mv *.sra.fastq ./unmated_reads
```

Go to [Table of Contents](#table-of-contents)

## 3. Alternative option to jointly download and convert SRA to fastq

We can combine the two steps above into a single one using the `fasterq-dump` command. However, for the amount of data I was working with (248 genomes!), this option was so memory demanding.

```bash
pavelmatos@nympha:~/eudaminae/data/janzen19/fastq$
cd ..

while read line;
do echo fasterq-dump $line -O /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq;
done < SRR_Acc_List.txt > Metacentrum_download_to_FASTQ.sh

nano Metacentrum_download_to_FASTQ.sh

>>> #PBS -N download2fastq_qsub
>>> #PBS -l select=1:ncpus=6:mem=200gb:scratch_local=200gb
>>> #PBS -l walltime=23:59:00
>>> 
>>> #clean scratch after the end
>>> trap 'clean_scratch' TERM EXIT
>>> 
>>> # go to scratch directory
>>> cd $SCRATCHDIR || exit 1
>>> 
>>> source /storage/plzen1/home/pavelmatos/.bashrc
>>> 
>>> fasterq-dump SRR7174339 -O /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
>>> fasterq-dump SRR7174340 -O /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
>>> etc ...
```

Go to [Table of Contents](#table-of-contents)
