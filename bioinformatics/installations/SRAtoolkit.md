# SRA (Sequence Read Archive) toolkit

We use this program to download NGS data from GenBank and convert SRA files to fastq format.

The documentation is available [here](https://github.com/ncbi/sra-tools/wiki/Downloads). By the time I downloaded SRA toolkit, the Ubuntu 64 version was 2.9.6-1. The latest versions for different OS is available [here](https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit).

```bash
pavelmatos@nympha:~$
cd ./software
wget http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.9.6-1/sratoolkit.2.9.6-1-ubuntu64.tar.gz
tar -xzf sratoolkit.2.9.6-1-ubuntu64.tar.gz
```

To get the commands of SRA toolkit working without always going back to the sratoolkit.2.9.6-1-ubuntu64/binbin folder is to add the SRA toolkit to the PATH. Just add the line below to the bottom of the .bashrc file at my home directory.

```bash
nano ~/.bashrc
## Add the following text to the last line of .bashrc
export PATH="/storage/plzen1/home/pavelmatos/software/sratoolkit.2.9.6-1-ubuntu64/bin:$PATH"
## Ctrl+x -> Save Y
```
