# SECAPR, Sequence CApture PRocessor
We use this pipeline to work with target sequence and whole-genome resequencing data, from cleaning to alignments

## Conda installation
Conda is an environment manager that makes installation of new software along with their required dependencies very simple and straightforward.

SECAPR is a collection of packages written in Python v2. We begin by downloading Python v2.7 using [Miniconda](https://docs.conda.io/en/latest/miniconda.html).

```console
pavelmatos@nympha:~$ mkdir software
pavelmatos@nympha:~$ cd software
pavelmatos@nympha:~/software$ wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
pavelmatos@nympha:~/software$ sh Miniconda2-latest-*.sh
```

Follow the directions. When asking whether to append the miniconda directory to $PATH, simply type yes. When it asks whether to allow the installer to initialize Miniconda2 by running conda init, type again yes.

When it asks in which location Miniconda2 should be installed, type: `/storage/plzen1/home/pavelmatos/software/miniconda2`.

Once the installation is completed, quit and reopen the terminal. It is important to test that conda is really installed. First, when openning a new session you need to type `source ~/.bashrc` to have conda in the $PATH.

```console
pavelmatos@nympha:~/software$ source ~/.bashrc
(base) pavelmatos@nympha:~/software$ conda info
```

Finally, add Bioconda channels containing relevant bioinformatic software.

```console
(base) pavelmatos@nympha:~/software$ conda config --add channels defaults; conda config --add channels conda-forge; conda config --add channels bioconda; conda config --add channels https://conda.anaconda.org/faircloth-lab
```

## SECAPR installation
Conda has automatically downloaded and installed all the necessary software dependencies. It's recommended that SECAPR and all its dependencies are in a separate **virtual environment**. This is important because the new environment will not interfere with potentially already installed versions of the software dependencies.

Install SECAPR in a new virtual environment named _secapr_env_.

```console
(base) pavelmatos@nympha:~/software$ conda create -n secapr_env secapr
```

### The active environment
Before working with SECAPR from now on, we need to activate its virtual environment using:

```console
(base) pavelmatos@nympha:~/software$ source activate secapr_env
```

When the environment is activated, all the necessary software dependencies are available in the standard $PATH.

For example, when you type `samtools`, the software's version required by SECAPR will be executed.

You can also check that the correct environment is activated by typing the following. There should be a * in front of _secapr_env_ in the output of this command.

```console
(base) pavelmatos@nympha:~/software$ conda info --envs
```

To deactivate the environment and to switch back to the standard environment, use the following:

```console
(base) pavelmatos@nympha:~/software$ conda deactivate
```

### Installation of the development version of SECAPR
I use the latest updates of SECAPR available only in GitHub and not in the package installation made by conda. Proceed to the following steps to get SECAPR development version

1. Activate the SECAPR environment

```console
pavelmatos@nympha:~/software$ source activate secapr_env
```
2. Download the latest updates as stored in GitHub

```console
(base) pavelmatos@nympha:~/software$ wget https://github.com/AntonelliLab/seqcap_processor/archive/master.zip
```

3. Unzip the file. The folder is in the ./software. However, when working in a different computer make sure to place the unzipped directory in a safe location where you won't delete it, for example, not on your `/desktop` or `/download` folders. This is because the directory will be in the $PATH where SECAPR will be executed from in the future.

```console
(base) pavelmatos@nympha:~/software$ unzip master.zip
```

4. Remove the current SECAPR installation made by conda

```console
(base) pavelmatos@nympha:~/software$ conda remove secapr
```

5. Go to the unzipped directory and install the development version of SECAPR using Python

```console
(base) pavelmatos@nympha:~/software$ cd seqcap_processor-master
(base) pavelmatos@nympha:~/software/seqcap_processor-master$ python -m pip install -e .
```

The new installation is located in the folder `./seqcap_processor-master` which **should not be deleted**.

To double-check that we have the correct development version:

```console
(base) pavelmatos@nympha:~/software/seqcap_processor-master$ cd ..
(base) pavelmatos@nympha:~/software$ source activate secapr_env
(secapr_env) pavelmatos@nympha:~/software$ secapr --version
secapr 0+unknown
```

#### When SECAPR has been previously installed in the cluster (e.g, Albiorix)
If this is happening, you won't have writing access to update SECAPR to the development version.

Another option to install SECAPR locally is to use the script `secapr_list_linux.txt` written by Tobias Andermann. The file is stored at my laptop `D:\PAVEL\Lepidoptera projects\Hesperiidae\Laboratory\NGS bioinformatics\src` and also in this GitHub notebook at `Eudaminae_phylogeny/bioinformatics/src`.

Execute the following command where you want to install SECAPR

```sh
conda create --name secapr --file ./secapr_list_linux.txt
```

From now on the name of the environment would be `secapr` and not `secapr_env`. Thus, to activate the pipeline, type `source activate secapr`.

