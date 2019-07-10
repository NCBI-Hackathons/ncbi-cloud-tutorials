# BLAST tutorial with Jupyter Notebooks and Command Line

The Basic Local Alignment Search Tool (BLAST) finds regions of local similarity between sequences. Using command line and Python-based features, we can do even more with BLAST. In this tutorial, we demonstrate how to use some of these features. We will use Jupyter notebooks that work in-browser, allowing you to skip setting up the proper environment locally. Click the `launch binder` button below to launch the Jupyter notebook.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/NCBI-Hackathons/ncbi-cloud-tutorials/master?filepath=%2FBLAST%20tutorials%2Findex.ipynb)


------

This tutorial is largely taken from [Wayne's Bioinformatics Code Portal's Blast Binder](https://github.com/fomightez/blast-binder). Most of the code and readme is from this source. It is your responsibility to ensure that your usage of software is in compliance with all applicable usage requirements.

The BLAST software is available directly from the National Library of Medicine under <a href="https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download">BLAST+ executables</a>.

The BLAST software references are listed [here](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=References).

NCBI has made available an Amazon Machine Image (AMI) that will generate a remote cloud compute instance already set to run standalone BLAST, see [here](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=CloudBlast). Additional documentation is [here](http://ncbi.github.io/blast-cloud/).

Here are some web-based BLAST resources:

* [NCBI's main BLAST page](https://blast.ncbi.nlm.nih.gov/Blast.cgi)
* [Saccharomyces Genome Database web interface](https://www.yeastgenome.org/blast-sgd)
* [SequenceServer](http://www.sequenceserver.com/) lets you set up a BLAST+ server for individual use or for sharing datasets and lists several community databases that use it for a querying mechanism.

Technical Details
-----------------

This repository is set up to make use of the binder service offered by [MyBinder.org](https://mybinder.org/). 

It was developed from combining much of the binderized repo [here](https://github.com/fomightez/qgrid-notebooks) with the ability to also run PatMatch. Click [here](https://github.com/fomightez/patmatch-binder) for information about PatMatch and launchable Jupyter notebooks demonstrating its use.
