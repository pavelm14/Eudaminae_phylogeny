## 2. Retrieve data from local disk to cluster (and vice versa)
I use MobaXterm to transfer data between my local computer and the MetaCentrum. For example, I want to back up a copy of the raw target capture sequencing data that is in my disk D:

```bash
[pavel local computer]$

cd /drives/d/PAVEL/Lepidoptera\ projects/Hesperiidae/Laboratory/RapidGenomics/data/raw/
```

There are 290 files with compressed sequences (.tar.gz) which correponds to the R1 and R2 reads for 145 skipper specimens.

To copy these recursiverly to my back up folder in `storage-brno3-cerit.metacentrum.cz`

```bash
[pavel local computer]$

scp -r ./*.fastq.gz pavelmatos@zuphux.cerit-sc.cz:/home/pavelmatos/eudaminae/raw
```

If many large directories are needed, a batch download of part of them might be better. In this case, I upload the first 30 specimens having clean (trimmed) reads. The sequences are stored uncompressed (.fastq) for each specimen in one numbered folder (from 001/ to 145/).

```bash
[pavel local computer]$

for i in {001..030}; 
do scp -r ../../../NGS\ bioinformatics/processed/2_cleaned_trimmed_reads/"$i"_clean/ pavelmatos@nympha.metacentrum.cz:/home/pavelmatos/eudaminae/processed/2_cleaned_trimmed_reads;
done
```

### Changing the rights (write, read, execute) in files and folders
After uploading files from my own computer to the cluster, the user/group at the cluster will not have rights to write nor execute the data. Therefore, I might need to change permissions in some cases.

To change modes to `user`, `group`, `others` to `write`, `read`, `execute` on a bunch of folders or files, the easiest way is to run the following example for directories:

```bash
pavelmatos@nympha:~$

find ./any_folder/ -type d -exec chmod 755 {} \;
```

And this is for files within a folder:

```bash
pavelmatos@nympha:~$

find ./any_folder/ -type f -exec chmod 755 {} \;
```

The modes can be changed using numbers. For example, `chmod 755` means full access (7: rwx) to the `user`, read and execute modes (5: r-x) to both the `group` and `others`. Another example, `chmod 644` means read and write access (6: rw-) to the `user`, and read-only access (4: r--) to both the `group` and `others`. You can [have a look](https://en.wikipedia.org/wiki/Chmod) to what the numbers, from 0 to 7, mean when using `chmod`.

Another way to change permits:

```bash
pavelmatos@nympha:~$
chmod +x file_rename.sh ##It makes the file_rename executable
```

### Transfer sanitation
If you want to check the size of a directory, use the `du` (disk usage) command with the options `-s` for summary and `-h` for human readable mode.

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


