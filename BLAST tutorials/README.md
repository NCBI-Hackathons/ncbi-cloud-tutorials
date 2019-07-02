# blast-binder

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nmarda/blast-binder/master?filepath=index.ipynb)

*tl;dr:*  
Click `launch binder` to run command line-based BLAST in your browser.

------

***BLAST: Basic Local Alignment Search Tool (BLAST+) command-line applications.***

This repository is for running BLAST+ in Jupyter environment provided by [MyBinder.org](https://mybinder.org/). It also incorporates Python-based features.

This tutorial is largely taken from [Wayne's Bioinformatics Code Portal's Blast Binder](https://github.com/fomightez/blast-binder). The following text is the software and usage attributions that he cites. It is your responsibility to ensure that your usage of software is in compliance with all applicable usage requirements.


# Readme text from Wayne's Blast Binder
-------

Software
--------

The BLAST software will be installed already in each active session launched from this repository. The BLAST software is available directly from the National Library of Medicine under <a href="https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download">BLAST+ executables</a>.


**BLAST is a registered trademark of the National Library of Medicine**, see [here](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=References).

The BLAST software references are listed [here](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=References).

Users of BLAST+ here should probably cite:

- BLAST+: architecture and applications. Camacho C., Coulouris G., Avagyan V., Ma N., Papadopoulos J., Bealer K., & Madden T.L. (2008) BMC Bioinformatics 10:421. [PMID:20003500](https://www.ncbi.nlm.nih.gov/pubmed/20003500?dopt=Citation)

- BLAST: a more efficient report with usability improvements. Boratyn GM, Camacho C, Cooper PS, Coulouris G, Fong A, Ma N, Madden TL, Matten WT, McGinnis SD, Merezhuk Y, Raytselis Y, Sayers EW, Tao T, Ye J, & Zaretskaya I. (2013) Nucleic Acids Res. 41:W29-W33.[PMID: 23609542](https://www.ncbi.nlm.nih.gov/pubmed/23609542)


***Clarifying Software Attribution: I, Wayne, am not involved in the BLAST software at all. Those at the National Library of Medicine are the developers and distributors of BLAST. See their materials. I simply set up this repository to make the software useable on the command line without installation headaches.***

I, Wayne, did code a Python-based utility for use with the results from command line BLAST; it is available [here](https://github.com/fomightez/sequencework/tree/master/blast-utilities) and utilized in the notebooks in this repository to process the results and allow easily converting the results to other forms.

Usage
-----

This repository is set up to allow running the command line version of BLAST software after pressing the `launch binder` button above or below. The target use case is when you want to run custom BLAST or process a lot of sequences.

In the notebooks that can be launched, I have added some examples illustrating how to use the program and process the results easily with Python and convert to other forms. **Additionally, useful resources for using command line BLAST are in those notebooks.** Alternatively, the notebook with most of resources can be viewed statically [here](https://nbviewer.jupyter.org/github/nmarda/blast-binder/blob/master/notebooks/BLAST%20on%20Command%20Line%20and%20Integrating%20with%20Python.ipynb). The ['Credits/Resources' section right at the top](https://nbviewer.jupyter.org/github/nmarda/blast-binder/blob/master/notebooks/BLAST%20on%20Command%20Line%20and%20Integrating%20with%20Python.ipynb) is a good place to start.

**The Binder-launchable version too limiting for your needs?**

NCBI has made available an Amazon Machine Image (AMI) that will generate a remote cloud compute instance already set to run standalone BLAST, see [here](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=CloudBlast). Additional documentation is [here](http://ncbi.github.io/blast-cloud/).

Obviously, web-based BLAST is also available. Here are some of my favorite web-based BLAST resources:

* [NCBI's main BLAST page](https://blast.ncbi.nlm.nih.gov/Blast.cgi)
* [Saccharomyces Genome Database web interface](https://www.yeastgenome.org/blast-sgd)
* [SequenceServer](http://www.sequenceserver.com/) lets you set up a BLAST+ server for individual use or for sharing datasets and lists several community databases that use it for a querying mechanism.


Technical Details
-----------------

This repository is set up to make use of the binder service offered by [MyBinder.org](https://mybinder.org/). See their site for more information about Binder.

Developed mainly from combining much of the binderized repo from [here](https://github.com/fomightez/qgrid-notebooks) with the ability to also run PatMatch, see [here](https://github.com/fomightez/patmatch-binder) for information about PatMatch and launchable Jupyter notebooks demonstrating its use.

I borrrowed the 'warning' highlight/introductory text about notebooks at the top of the included notebook from Tim Sherratt's notebook [here](https://github.com/GLAM-Workbench/te-papa-api/blob/master/Exploring-the-Te-Papa-collection-API.ipynb).

Click this button below to begin using BLAST (or PatMatch, as well):

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nmarda/blast-binder/master?filepath=index.ipynb)
