History
=======

1.1.0 (2022-10-20)
------------------

* Renames the CDS output files to *.faa for amino acids and *.ffn for nulceotide sequences
* Implementation of consistent CDS name (equal to the locus_tag) across all output files
* Creates terL.faa and terL.ffn, which contain the sequences of any identified terminase large subunit CDSs
* Passes multithreading to PHANOTATE and tRNAscan-SE in meta mode indicated by flag -m, which provides approximately a t-fold improvement in run-time for large metavirome datasets, where t is the number of threads. 

1.0.1 (2022-10-10)
------------------

* Minor release to fix a string-parsing bug where pharokka v1.0.0 would crash when certain VFDB virulence factors were detected.

1.0.0 (2022-09-16)
------------------

* Removes errors (with post_processing functions not being parsed as strings) to improve robustness.
* Codebase more reliable and consistent
* Overhaul of install_databases.py
* Adds pre-existing Pharokka Database available at https://zenodo.org/record/7081772

0.1.11 (2022-09-13)
------------------

*  Adds CARD and VFDB databases.

0.1.10 (2022-08-31)
------------------

* Fixes issues with Genbank output files.


0.1.9 (2022-08-04)
------------------

* Fixes bug with parsing Aragorn output for some phages.
* Fixes bug with phages with no mmseqs2 PHROGs hits (for some very small phages).

0.1.8 (2022-07-24)
------------------

* Minor release.
* Fixes bug with install_databases.py.
* Adds locustag to .tbl output.

0.1.7 (2022-07-24)
------------------

* Removes hh-suite dependency to reduce run-time (redundant with e-value option).
* Adds e-value option for passing into mmseqs2.
* Adds CRISPR detection using MinCED.
* Adds tmRNA detection using Aragorn.
* Adds CDS coding density to _length_gc_cds_density.tsv.

0.1.6 (2022-07-16)
------------------

* Version update for bioconda release

0.1.5 (2022-07-09)
------------------

* Adds Prodigal option for gene prediction -g prodigal
* Handles metagenome assembled virus contigs (that may have no genes) with -m flag
* Fixes tRNA count bug in cds_functions.tsv

0.1.4 (2022-07-08)
------------------

* Updates PHROGs annotation to version 4.

0.1.3 (2022-07-06)
------------------

* Adds .log file
* Adds -l option

0.1.2 (2022-07-04)
------------------

* Adds .genbank file

0.1.1 (2022-06-28)
------------------

* Second release

0.1.0 (2021-12-09)
------------------

* First release
