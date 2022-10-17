
pharokka
===============

Fast Phage Annotation Program
------------

pharokka is designed for rapid standardised annotation of bacteriophages.

If you are looking for rapid standardised annotation of prokaryotes, please use [prokka](https://github.com/tseemann/prokka), which inspired the creation of pharokka.

Table of Contents
-----------
- [pharokka](#pharokka)
  - [Fast Phage Annotation Program](#fast-phage-annotation-program)
  - [Table of Contents](#table-of-contents)
- [Method](#method)
- [Installation](#installation)
- [Beginner Conda Installation](#beginner-conda-installation)
- [Usage](#usage)
- [Version Log](#version-log)
- [System](#system)
- [Time](#time)
- [Bugs and Suggestions](#bugs-and-suggestions)
- [Citation](#citation)

# Method

![pharokka workflow](img/pharokka_workflow.png?raw=true "Pharokka Workflow")

Briefly, default gene prediction is done using PHANOTATE (https://github.com/deprekate/PHANOTATE) and function annotation is based on the PHROGs database (https://phrogs.lmge.uca.fr) with mmseqs2.

The main output is a *.gff file that is suitable for use downstream pangenomic pipelines such as Roary (https://sanger-pathogens.github.io/Roary/).

The other important output is `cds_functions.tsv`, which includes counts of CDSs, tRNAs, tmRNAs, CRISPRs and functions assigned to CDSs according to the PHROGs database.

For full documentation of output files, please visit https://pharokka.readthedocs.io.

# Installation

**pharokka v1.0.0 is now available on bioconda**

* v1.0.0 adds VFDB (current as of 15-09-22) and CARD (v3.2.4) databases for virulence factor and AMR gene identification.
* These should install using the install_databases.py script, with the databases downloaded from a Zenodo repository.
* You will need to re-install the databases if you updating from an earlier version of pharokka.
* If the script does not work, you an alternatively download the databases manually from Zenodo at https://zenodo.org/record/7081772/files/pharokka_database_v1.0.0.tar.gz and untar the directory in a location of your choice. Please see the Installation Section for more details.

The easiest way to install pharokka is via conda. For inexperienced command line users, this method is highly recommended.

`conda install -c bioconda pharokka`

This will install all the dependencies along with pharokka. The dependencies are listed in environment.yml.

If conda is taking a long time to solve the environment, try using mamba:

```
conda install mamba
mamba install -c bioconda pharokka
```

Alternatively, the development version of pharokka can be installed manually via github. 

`git clone https://github.com/gbouras13/pharokka.git`

The dependencies found in environment.yml will then need to be installed manually.

For example using conda to install the required dependencies:

```
git clone https://github.com/gbouras13/pharokka.git
cd pharokka
conda env create -f environment.yml
conda activate pharokka_env
```

And then to run pharokka (assuming you are still in the pharokka directory)

```
./bin/install_databases.py -h
./bin/pharokka.py -h
```

# Beginner Conda Installation

If you are new to using the command-line, please install conda using the following instructions.

1. Install [Anaconda](https://www.anaconda.com/products/distribution). I would recommend [miniconda](https://docs.conda.io/en/latest/miniconda.html).
2. Assuming you are using a Linux x86_64 machine (for other architectures, please replace the URL with the appropriate one on the [miniconda](https://docs.conda.io/en/latest/miniconda.html) website).

`curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

For Mac (Intel, will also work with M1):

`curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh`

3. Install miniconda and follow the prompts.

`sh Miniconda3-latest-Linux-x86_64.sh`

4. After installation is complete, you should add the following channels to your conda configuration:

```
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

5. After this, conda should be installed (you may need to restart your terminal). It is recommended that mamba is also installed, as it will solve the enviroment quicker than conda:

`conda install mamba`

 6. Finally, I would recommend installing pharokka into a fresh environment. For example to create an environment called pharokkaENV with pharokka installed:

```
mamba create -n pharokkaENV pharokka
conda activate pharokkaENV
install_databases.py -h
pharokka.py -h
```

# Usage

First the PHROGs databases need to be installed

`install_databases.py -d`

If you would like to specify a different database directory (recommended), that can be achieved as follows:

`install_databases.py -o <path/to/databse_dir>`

v1.0.0 adds VFDB and CARD databases for virulence factor and AMR gene identification. These should install using the install_databases.py script as outlined above. You will need to run this before running pharokka v1.0.0.

If this does not work, you an alternatively download the databases from Zenodo at https://zenodo.org/record/7081772/files/pharokka_database_v1.0.0.tar.gz and untar the directory in a location of your choice.

If you prefer to use the command line:

```
wget "https://zenodo.org/record/7081772/files/pharokka_database_v1.0.0.tar.gz"
tar -xzf pharokka_database_v1.0.0.tar.gz
```

which will create a directory called "pharokka_database_v1.0.0" containing the databases.

If you have already downloaded databases for earlier versions of pharokka, these will need to be re-downloaded.

Once the databases have finished downloading, to run pharokka

`pharokka.py -i <fasta file> -o <output folder> -t <threads>`

To specify a prefix for the output files:

`pharokka.py -i <fasta file> -o <output folder> -t <threads> -p <prefix>`

To specify a different database directory (recommended):

`pharokka.py -i <fasta file> -o <output folder> -d <path/to/database_dir> -t <threads> -p <prefix>`

To overwrite an existing output directory, use -f

`pharokka.py -i <fasta file> -o <output folder> -d <path/to/database_dir> -t <threads>  -p <prefix> -f`

To use Prodigal instead of PHANOTATE use `-g prodigal`

`pharokka.py -i <fasta file> -o <output folder> -d <path/to/database_dir> -t <threads>  -g prodigal`

pharokka should work with metagenome assembled viral contigs with PHANOTATE automatically. With prodigal, please add the -m flag

`pharokka.py -i <fasta file> -o <output folder> -d <path/to/database_dir> -t <threads>  -g prodigal -m`

In v0.1.7, the ability to specify an E-value threshold for PHROGs CDS functional assignment using mmseqs2 was added using the -e flag. It defaults to 1E-5.

`pharokka.py -i <fasta file> -o <output folder> -d <path/to/database_dir> -t <threads>  -e <E-value>`

pharokka defaults to 1 thread.

```
usage: pharokka.py [-h] -i INFILE [-o OUTDIR] [-d DATABASE] [-t THREADS] [-f] [-p PREFIX] [-l LOCUSTAG] [-g GENE_PREDICTOR] [-m] [-c CODING_TABLE] [-e EVALUE] [-V]

pharokka: fast phage annotation program

optional arguments:
  -h, --help            show this help message and exit
  -i INFILE, --infile INFILE
                        Input genome file in fasta format.
  -o OUTDIR, --outdir OUTDIR
                        Directory to write the output to.
  -d DATABASE, --database DATABASE
                        Database directory. If the databases have been installed in the default directory, this is not required. Otherwise specify the path.
  -t THREADS, --threads THREADS
                        Number of threads for mmseqs and hhsuite. Defaults to 1.
  -f, --force           Overwrites the output directory.
  -p PREFIX, --prefix PREFIX
                        Prefix for output files. This is not required.
  -l LOCUSTAG, --locustag LOCUSTAG
                        User specified locus tag for the gff/gbk files. This is not required. A random locus tag will be generated instead.
  -g GENE_PREDICTOR, --gene_predictor GENE_PREDICTOR
                        User specified gene predictor. Use "-g phanotate" or "-g prodigal". Defaults to phanotate (not required unless prodigal is desired).
  -m, --meta            Metagenomic option for Prodigal
  -c CODING_TABLE, --coding_table CODING_TABLE
                        translation table for prodigal. Defaults to 11. Experimental only.
  -e EVALUE, --evalue EVALUE
                        E-value threshold for mmseqs2 PHROGs database search. Defaults to 1E-05.
  -V, --version         Version
  ```

# Version Log

A brief description of what is new in each update of pharokka can be found in the HISTORY.md file.

# System

pharokka has been tested on Linux and MacOS (M1 and Intel).

# Time

On a standard 16GB RAM laptop specifying 8 threads, pharokka should take between 3-10 minutes to run for a single phage, depending on the genome size.

# Bugs and Suggestions

If you come across bugs with pharokka, or would like to make any suggestions to improve the program, please open an issue or email george.bouras@adelaide.edu.au

# Citation

If you use pharokka, please also cite:

* McNair K., Zhou C., Dinsdale E.A., Souza B., Edwards R.A. (2019) "PHANOTATE: a novel approach to gene identification in phage genomes", Bioinformatics, https://doi.org/10.1093/bioinformatics/btz26.
* Chan, P.P., Lin, B.Y., Mak, A.J. and Lowe, T.M. (2021) "tRNAscan-SE 2.0: improved detection and functional classification of transfer RNA genes", Nucleic Acids Res., https://doi.org/10.1093/nar/gkab688.
* Steinegger M. and Soeding J. (2017), "MMseqs2 enables sensitive protein sequence searching for the analysis of massive data sets", Nature Biotechnology https://doi.org/10.1038/nbt.3988.
* Terzian P., Olo Ndela E., Galiez C., Lossouarn J., Pérez Bucio R.E., Mom R., Toussaint A., Petit M.A., Enault F., "PHROG : families of prokaryotic virus proteins clustered using remote homology", NAR Genomics and Bioinformatics, (2021), https://doi.org/10.1093/nargab/lqab067.
* Bland C., Ramsey L., Sabree F., Lowe M., Brown K., Kyrpides N.C., Hugenholtz P. , "CRISPR Recognition Tool (CRT): a tool for automatic detection of clustered regularly interspaced palindromic repeats", BMC Bioinformatics, (2007), https://doi.org/10.1186/1471-2105-8-209.
* Laslett D., Canback B., "ARAGORN, a program to detect tRNA genes and tmRNA genes in nucleotide sequences.", Nucleic Acids Research (2004) https://doi.org/10.1093/nar/gkh152.
* Chen L., Yang J., Yao Z., Sun L., Shen Y., Jin Q., "VFDB: a reference database for bacterial virulence factors", Nucleic Acids Research (2005) https://doi.org/10.1093/nar/gki008.
* Alcock et al, "CARD 2020: antibiotic resistome surveillance with the comprehensive antibiotic resistance database." Nucleic Acids Research (2020) https:doi.org/10.1093/nar/gkz935.