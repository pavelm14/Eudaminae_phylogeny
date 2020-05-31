# IQ-TREE
We use this program to infer maximum likelihood phylogenomic trees. The main advantages of this software is the speed and accuracy when inferring phylogenies using hundreds of loci. IQ-TREE also integrates ModelFinder so that substitution model is estimated during the analysis.

You can find the latest releases [here](https://github.com/Cibiv/IQ-TREE/releases). As of May 16, 2020, the latest stable release was 2.0.5.

```bash
pavelmatos@nympha:~$
cd ~/software

wget https://github.com/Cibiv/IQ-TREE/releases/download/v2.0.5/iqtree-2.0.5-Linux.tar.gz
tar -zxvf iqtree-2.0.5-Linux.tar.gz

rm iqtree-2.0.5-Linux.tar.gz
```

For convenience, I added the `bin` directory of IQ-TREE to my $PATH so that I can use the commands directly on the screen.

```bash
pavelmatos@nympha:~$
cd ~
nano .bashrc

## Add the following text to the last line of .bashrc
export PATH="/storage/plzen1/home/pavelmatos/software/iqtree-2.0.5-Linux/bin:$PATH"
## Ctrl+x -> Save Y
```

Next time for running IQTREE 2, you just need to do the following:

```bash
pavelmatos@nympha:~$
source ~/.bashrc
iqtree2 -h
```

I also previously installed the version 1 of IQTREE. As on March 23, 2020, the latest stable release was 1.6.12.

```bash
pavelmatos@nympha:~$
cd ~/software
wget https://github.com/Cibiv/IQ-TREE/releases/download/v1.6.12/iqtree-1.6.12-Linux.tar.gz
tar -zxvf iqtree-1.6.12-Linux.tar.gz
rm iqtree-1.6.12-Linux.tar.gz
```

For convenience, I also added the `bin` directory of IQ-TREE to my $PATH so that I can use the commands directly on the screen.

```bash
pavelmatos@nympha:~$
cd ~
nano .bashrc
## Add the following text to the last line of .bashrc
export PATH="/storage/plzen1/home/pavelmatos/software/iqtree-1.6.12-Linux/bin:$PATH"
## Ctrl+x -> Save Y
```

Next time for running IQTREE 1, you just need to do the following:

```bash
pavelmatos@nympha:~$
source ~/.bashrc
iqtree -h
```
