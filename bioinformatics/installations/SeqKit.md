# SeqKit toolkit

We use this program to manipulate fasta files.

The documentation is available [here](https://bioinf.shenwei.me/seqkit/). By the time I downloaded SeqKit toolkit, the Linux version was 0.12.0. The latest versions for different OS is available [here](https://github.com/shenwei356/seqkit/releases).

```bash
pavelmatos@nympha:~$ 
cd ./software
mkdir seqkit && cd "$_"

wget https://github.com/shenwei356/seqkit/releases/download/v0.12.0/seqkit_linux_386.tar.gz
tar -zxvf seqkit_linux_386.tar.gz
```

We will add `seqkit` to the PATH, just add the line below to the bottom of the .bashrc file at home directory.

```bash
pavelmatos@nympha:~/software/seqkit$ 
nano ~/.bashrc

## Add the following text to the last line of .bashrc
export PATH="/storage/plzen1/home/pavelmatos/software/seqkit:$PATH"
## Ctrl+x -> Save Y
```

Now, it's possible to use the program from the PATH environment.

```bash
pavelmatos@nympha:~/software/seqkit$ 
source ~/.bashrc

seqkit help
```

### References
- Shen et al. (2016) SeqKit: A cross-platform and ultrafast toolkit for FASTA/Q file manipulation. PLOS ONE 11(10): e0163962. DOI: [10.1371/journal.pone.0163962](https://doi.org/10.1371/journal.pone.0163962)
