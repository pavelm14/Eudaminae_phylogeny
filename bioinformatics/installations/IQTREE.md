# IQ-TREE
We use this program to infer maximum likelihood phylogenomic trees. The main advantages of this software is the speed and accuracy when inferring phylogenies using hundreds of loci. IQ-TREE also integrates ModelFinder so that substitution model is estimated during the analysis.

You can find the latest releases [here](https://github.com/Cibiv/IQ-TREE/releases). As on March 23, 2020, the latest stable release was 1.6.12.

```bash
pavelmatos@nympha:~$
cd ~/software
wget https://github.com/Cibiv/IQ-TREE/releases/download/v1.6.12/iqtree-1.6.12-Linux.tar.gz
tar -zxvf iqtree-1.6.12-Linux.tar.gz
rm iqtree-1.6.12-Linux.tar.gz
```

For convenience, I added the `bin` directory of IQ-TREE to my $PATH so that I can use the commands directly on the screen.

```bash
pavelmatos@nympha:~$
cd ~
nano .bashrc
```
