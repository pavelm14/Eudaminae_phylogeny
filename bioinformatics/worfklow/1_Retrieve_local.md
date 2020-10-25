# Retrieve data from local disk to cluster (and vice versa)
I use MobaXterm to transfer data between my local computer and the MetaCentrum. Here, we will retrieve the target capture data that is stored at my local compututer. The company RapidGenomics sent the sequences by a private link but this is short-lived, thus, all data is safely stored in different locations, including my local computer.

##### Table of Contents
- [Software](#software-that-we-need)  
1. [Copy data from local to cluster](#1-copy-raw-data-from-local-computer)
2. [Change rights to documents](#2-changing-the-rights-of-documents)
3. [Transfer sanitation](#3-transfer-sanitation)

## Software that we need
- [MobaXterm](https://mobaxterm.mobatek.net/download.html): is a very convenient interface for Windows OS. The syntax it uses is bash. Download it from [here](https://mobaxterm.mobatek.net/download.html) (the Home free edition is enough for our purposes).

## 1. Copy raw data from local computer

In this exercise, I will back up a copy of the raw target capture sequencing data that is in my local disk D:

```bash
[pavel local computer]$

cd /drives/d/PAVEL/Lepidoptera\ projects/Hesperiidae/Laboratory/RapidGenomics/data/raw/
```

There are 290 files with compressed sequences (.tar.gz) which correponds to the R1 and R2 reads for 145 skipper specimens.

To copy these recursiverly into my back up folder at `storage-brno3-cerit.metacentrum.cz`

```bash
[pavel local computer]$

scp -r ./*.fastq.gz pavelmatos@zuphux.cerit-sc.cz:/home/pavelmatos/eudaminae/raw
```

If many large directories are needed, a batch download is better. In this case, I retrieve the first 30 specimens having clean (trimmed) reads. The sequences are stored uncompressed (.fastq) for each specimen in one numbered folder (from 001/ to 145/).

```bash
[pavel local computer]$

for i in {001..030}; 
do scp -r ../../../NGS\ bioinformatics/processed/2_cleaned_trimmed_reads/"$i"_clean/ pavelmatos@nympha.metacentrum.cz:/home/pavelmatos/eudaminae/processed/2_cleaned_trimmed_reads;
done
```

## 2. Changing the rights of documents
After uploading files from my own computer to the cluster, the user/group at the cluster will have no rights to write nor execute the data. Therefore, I need to change permissions in these cases.

To change modes to `user`, `group`, `others` to `write`, `read`, `execute` on a bunch of folders or files, the easiest way is to run the following example for directories.

```bash
[pavel local computer]$

ssh -x pavelmatos@nympha.metacentrum.cz

#At nympha frontend
pavelmatos@nympha:~$

find ./any_folder/ -type d -exec chmod 755 {} \;
```

And this is for files within a folder:

```bash
pavelmatos@nympha:~$

find ./any_folder/ -type f -exec chmod 755 {} \;
```

The modes can be changed using numbers. For example, `chmod 755` means full access (7: rwx) to the `user`, read and execute modes (5: r-x) to both the `group` and `others`.

Another example, `chmod 644` means read and write access (6: rw-) to the `user`, and read-only access (4: r--) to both the `group` and `others`.

You can [have a look](https://en.wikipedia.org/wiki/Chmod) to what the numbers, from 0 to 7, mean when using `chmod`.

Another way to change permits:

```bash
pavelmatos@nympha:~$

chmod +x file_rename.sh ##It makes the file_rename executable for all users
```

## 3. Transfer sanitation
If you want to check the size of a transferred directory, use the `du` (disk usage) command with the options `-s` for summary and `-h` for human readable mode.

```bash
pavelmatos@nympha:~$

du -sh /storage/plzen1/home/pavelmatos/eudaminae/data/janzen19/fastq
```

It is also a good idea to check the file integrity after transfering data to the cluster.

```bash
pavelmatos@nympha:~$

cd eudaminae/data/janzen19/fastq

md5sum * > checklist.chk ##Summarize results in the .chk file
md5sum -c checklist.chk ##Report integrity of every file
```

Some files that were created in Windows can be screwed or not in the correct format fully understandable in UNIX. It is advisable to change the format in files created in another OS.

```bash
pavelmatos@nympha:~$

dos2unix file_rename.sh
```


