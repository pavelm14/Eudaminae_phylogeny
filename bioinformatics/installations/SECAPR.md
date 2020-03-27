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
