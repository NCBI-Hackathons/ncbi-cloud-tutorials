# BLAST+ Tutorial

The [NCBI BLAST+ program](https://blast.ncbi.nlm.nih.gov) finds regions of local similarity between sequences. It has many features, which are detailed [here](https://www.ncbi.nlm.nih.gov/books/NBK279690/).

Recent advances in cloud computing have led to better pipelines for collecting, storing, and analyzing data. You can also take advantage of faster compute times and larger databases by using BLAST+ in the cloud.

This tutorial assumes that you have an account with Google Cloud Platform (GCP), as well as familiarity with its basic tools. If you do not, please see our [GCP tutorial](/~/tutorials/tutorial_gcp/).

In this tutorial, we will:

- [Install Docker](#installing-docker)
- [Learn tips for using Docker](#tips-for-using-docker)
- [Run BLAST](#run-blast)
- [Using Blast at production scale](#using-blast-at-production-scale)
- [View additional resources](#additional-resources)




# Use BLAST+ in the Cloud
[Docker](https://www.docker.com/) is a virtualization tool. An image is created, providing an analytical environment encapsulating applications and dependencies. This image can be saved and easily shared for others to recreate the same analytical environment across platforms and operating systems. A container is a runtime instance of an image. By using containerization, users can bypass the numerous steps required to compile, configure, and install a Unix-based tool like BLAST+. Additionally, containerization is a lightweight approach to make analysis more findable, accessible, interoperable, reusable (F.A.I.R.).  

## Installing Docker
Run the following commands to install Docker and add non-root users to run Docker.
```
sudo snap install docker
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker $USER
exit
# exit and SSH back in for changes to take effect
```
To confirm the correct installation of Docker, run the command `docker run hello-world`. If correctly installed, you should see ["Hello from Docker!..."](https://docs.docker.com/samples/library/hello-world/)  

## Tips for using Docker
Here are some helpful tips for using Docker. This section is nonessential for executing the rest of the tutorial. 

### Docker run command options
Below is a list of `docker run` command line [options](https://docs.docker.com/engine/reference/commandline/run/) used in this tutorial.

| Name, short-hand (if available) | Description |
| :----------------------------  | :---------- |
|`--rm`|Automatically remove the container when it exits|
|`--volume` , `-v`|Bind mount a volume|
|`--workdir` , `-w`| Working directory inside the container|

### Docker run command structure
Here we briefly show how a Docker command works. The following command consists of three parts.

```
docker run --rm ncbi/blast \
```
```
    -v $HOME/blastdb_custom:/blast/blastdb_custom:rw \
    -v $HOME/fasta:/blast/fasta:ro \
    -w /blast/blastdb_custom \
```
```
    makeblastdb -in /blast/fasta/nurse-shark-proteins.fsa -dbtype prot \
    -parse_seqids -out nurse-shark-proteins -title "Nurse shark proteins" \
    -taxid 7801 -blastdb_version 5
```

The first part of the command `docker run --rm ncbi/blast` is an instruction to run the docker image `ncbi/blast` and remove the container when the run is completed.  
  
The second part of the command makes the query sequence data accessible in the container. [Docker bind mounts]( https://docs.docker.com/storage/bind-mounts/) uses `-v` to mount the local directories to directories inside the container and provide access permission rw (read and write) or ro (read only). 

Assuming your subject sequences are stored in the $HOME/fasta directory on the local host, you can use the parameter `-v $HOME/fasta:/blast/fasta:ro` to make that directory accessible inside the container in /blast/fasta as a read-only directory. The `-w /blast/blastdb_custom` flag sets the working directory inside the container.
  
The third part of the command is the BLAST+ command. In this case, it is executing makeblastdb to create BLAST database files.  
  
You can start an interactive bash session for this image by using `docker run -it ncbi/blast /bin/bash`. For the BLAST+ Docker image, the executables are in the folder /blast/bin and /root/edirect and added to the variable $PATH.
  
For additional information on the `docker run` command, please refer to [documentation](https://docs.docker.com/engine/reference/commandline/run/).

### Useful Docker commands
  
| Docker Command | Description |
| :----------------------------  | :---------- |
|`docker ps -a`|Displays a list of containers|
|`docker rm $(docker ps -q -f status=exited)`|Removes all exited containers, if you have at least 1 exited container|
|`docker rm <CONTAINER_ID>`|Removes a container|
|`docker images`|Displays a list of images|
|`docker rmi <REPOSITORY (IMAGE_NAME)>`|Removes an image|

### Using BLAST+ with Docker
  
This Docker image lets you run BLAST+ in an isolated container, facilitating reproducibility of BLAST results. As a user of this Docker image, you are expected to provide BLAST databases and query sequences to run BLAST
as well as a location outside the container to save the results. The following is a list of directories used by BLAST+. 
  
| Directory | Purpose | Notes |
| --------- | ------  | ----- |
| `$HOME/blastdb` | Stores NCBI-provided BLAST databases | If set to a _single, absolute_ path, the `$BLASTDB` environment variable could be used instead (see [Configuring BLAST via environment variables](https://www.ncbi.nlm.nih.gov/books/NBK279695/#_usermanual_Configuring_BLAST_via_environ_).) |
| `$HOME/queries` | Stores user-provided query sequence(s) | |
| `$HOME/fasta`   | Stores user-provided FASTA sequences to create BLAST database(s) | |
| `$HOME/results` | Stores BLAST results | Mount with `rw` permissions |
| `$HOME/blastdb_custom` | Stores user-provided BLAST databases | |

### Versions of BLAST Docker image
  
The following command displays the latest BLAST version.  
```docker run --rm ncbi/blast blastn -version```

Appending a tag to the image name (`ncbi/blast`) allows you to use a
different version of BLAST+ (see “Supported Tags and Respective Release Notes” section for supported versions).  

Different versions of BLAST+ exist in different Docker images. The following command will initiate download of the BLAST+ version 2.7.1 Docker image. 
```
docker run --rm ncbi/blast:2.7.1 blastn -version
## Display a list of images
docker images
```

For example, to use the BLAST+ version 2.7.1 Docker image instead of the latest version, replace the first part of the command

```docker run --rm ncbi/blast``` with ```docker run --rm ncbi/blast:2.7.1```

## Import sequences and create a BLAST database
We will fetch query and database sequences and then create a custom BLAST database.

```
# Start in a directory where you want to perform the analysis
## Create directories for analysis
cd ; mkdir blastdb queries fasta results blastdb_custom

## Retrieve query sequences
docker run --rm ncbi/blast efetch -db protein -format fasta \
    -id P01349 > queries/P01349.fsa
    
## Retrieve database sequences
docker run --rm ncbi/blast efetch -db protein -format fasta \
    -id Q90523,P80049,P83981,P83982,P83983,P83977,P83984,P83985,P27950 \
    > fasta/nurse-shark-proteins.fsa
    
## Make BLAST database 
docker run --rm \
    -v $HOME/blastdb_custom:/blast/blastdb_custom:rw \
    -v $HOME/fasta:/blast/fasta:ro \
    -w /blast/blastdb_custom \
    ncbi/blast \
    makeblastdb -in /blast/fasta/nurse-shark-proteins.fsa -dbtype prot \
    -parse_seqids -out nurse-shark-proteins -title "Nurse shark proteins" \
    -taxid 7801 -blastdb_version 5
```

To examine the newly created BLAST database, running the following command will display the accessions, lengths, and common name of the sequences in the database.

```
docker run --rm \
    -v $HOME/blastdb:/blast/blastdb:ro \
    -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
    ncbi/blast \
    blastdbcmd -entry all -db nurse-shark-proteins -outfmt "%a %l %T"
```

Alternatively, you can also download preformatted BLAST databases from NCBI or the NCBI Google storage bucket.

### View NCBI databases

```docker run --rm ncbi/blast update_blastdb.pl --showall --source ncbi```  

### View NCBI Google Cloud bucket databases

```docker run --rm ncbi/blast update_blastdb.pl --showall pretty --source gcp```

For a detailed description of `update_blastdb.pl`, please refer to the [documentation.](https://www.ncbi.nlm.nih.gov/books/NBK537770/)

### View local databases
The command below mounts the `$HOME/blastdb` path on the local machine as
`/blast/blastdb` on the container, and `blastdbcmd` shows the available BLAST
databases at this location.  

```
## Download Protein Data Bank Version 5 database (pdb_v5)
docker run --rm \
     -v $HOME/blastdb:/blast/blastdb:rw \
     -w /blast/blastdb \
     ncbi/blast \
     update_blastdb.pl --source gcp pdb_v5

## Display database(s) in $HOME/blastdb
docker run --rm \
    -v $HOME/blastdb:/blast/blastdb:ro \
    ncbi/blast \
    blastdbcmd -list /blast/blastdb -remove_redundant_dbs
```
  
You should see an output `/blast/blastdb/pdb_v5 Protein`.  
  
```
## For the custom BLAST database used in this example -
docker run --rm \
    -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
    ncbi/blast \
    blastdbcmd -list /blast/blastdb_custom -remove_redundant_dbs
```
You should see an output `/blast/blastdb_custom/nurse-shark-proteins Protein`.  

## Run BLAST
When running BLAST in a Docker container, note the mounts specified to the `docker run` command to make the input and outputs accessible. In the examples below, the first two mounts provide access to the BLAST databases, the third mount provides access to the query sequences, and the fourth mount provides a directory to save the results. (Note the `:ro` and `:rw` options, which mount the directories as read-only and read-write respectively.)  

```
docker run --rm \
    -v $HOME/blastdb:/blast/blastdb:ro \
    -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
    -v $HOME/queries:/blast/queries:ro \
    -v $HOME/results:/blast/results:rw \
    ncbi/blast \
    blastp -query /blast/queries/P01349.fsa -db nurse-shark-proteins \
    -out /blast/results/blastp.out
```  

You should see the output file ```$HOME/results/blastp.out```. With your query, BLAST identified the protein sequence P80049.1 as a match with a score of 14.2 and an E-value of 0.96. To view the content of this output file, use the command ```more $HOME/results/blastp.out```.

## Stop the GCP instance
Remember to [stop](https://cloud.google.com/compute/docs/instances/stop-start-instance) or [delete](https://cloud.google.com/compute/docs/instances/stop-start-instance) the VM to prevent incurring additional cost. You can do this at the GCP Console as shown below.

![GCP instance stop](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/gcp-instance-stop.png "GCP instance stop")

## Using BLAST at Production Scale
One of the benefits of cloud computing is scalability. In this section, we will demonstrate how to use the BLAST+ Docker image at production scale on GCP. We will perform a BLAST analysis similar to the approach described in this [publication](https://www.ncbi.nlm.nih.gov/pubmed/31040829) to compare de novo aligned contigs from bacterial 16S-23S sequencing against the nucleotide collection (nt) database.

To test scalability, we will estimate the time it will take to download and analyze nucleotide collection databases of different sizes. Expected results are summarized in the following tables.

Input files: 28 samples (multi-FASTA files) containing de novo aligned contigs from the publication.  
(Instructions to [download]((https://figshare.com/s/729b346eda670e9daba4)) and create the input files are described in the [code block](#commands-to-run) below.)    
  
Database: Pre-formatted BLAST nucleotide collection database, version 5 (nt_v5): 68.7217 GB  
  
|       | Input file name | File content | File size | Number of sequences | Number of nucleotides | Expected output size |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Analysis 1 | query1.fa | only sample 1 | 59 KB | 121 | 51,119 | 3.1 GB |
| Analysis 2 | query5.fa | only samples 1-5 | 422 KB | 717 | 375,154 | 10.4 GB |
| Analysis 3 | query.fa | all 28 samples | 2.322 MB | 3798 | 2,069,892 | 47.8 GB |

## BLAST+ Docker image benchmarks  
| VM Type/Zone | CPU | Memory (GB) | Hourly Cost* | Download nt (min) | Analysis 1 (min) | Analysis 2 (min) | Analysis 3 (min)| Total Cost**
| :-: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--: |
| n1-standard-8 us-east4c | 8 | 30 | $0.312 | 9 | 22 | - | - | - |
| n1-standard-16 us-east4c | 16 | 60 | $0.611 | 9 | 14 | 53 | 205 | $2.86 |
| n1-highmem-16 us-east4c | 16 | 104 | $0.767 | 9 | 9 | 30 | 143 | $2.44 |
| n1-highmem-16 us-west2a  | 16 | 104 | $0.809 | 11 | 9 | 30 | 147 | $2.60 |
| n1-highmem-16 us-west1b | 16 | 104 | $0.674 | 11 | 9 | 30 | 147 | $2.17 |
| BLAST website (blastn) | - | - | - | - |Searches exceed current restrictions on usage|Searches exceed current restrictions on usage|Searches exceed current restrictions on usage| - |


All GCP instances are configured with 200 GB of persistent standard disk. 

*Hourly costs were estimated in May 2019 when the VMs were created. They may no longer be accurate. It is your responsibility to monitor and manage your operations and costs.
   
Please refer to GCP for more information on [machine types](https://cloud.google.com/compute/docs/machine-types),
[regions and zones,](https://cloud.google.com/compute/docs/regions-zones/) and [compute cost.](https://cloud.google.com/compute/pricing)

## Commands to run
```
## Install Docker if not already done
## This section assumes using recommended hardware requirements below
## 16 CPUs, 104 GB memory and 200 GB persistent hard disk

## Modify the number of CPUs (-num_threads) in Step 3 if another type of VM is used.

## Step 1. Prepare for analysis
## Create directories
cd ; mkdir -p blastdb queries fasta results blastdb_custom

## Import and process input sequences
sudo apt install unzip
wget https://ndownloader.figshare.com/articles/6865397?private_link=729b346eda670e9daba4 -O fa.zip
unzip fa.zip -d fa

### Create three input query files
### All 28 samples
cat fa/*.fa > query.fa

### Sample 1
cat fa/'Sample_1 (paired) trimmed (paired) assembly.fa' > query1.fa

### Sample 1 to Sample 5
cat fa/'Sample_1 (paired) trimmed (paired) assembly.fa' \
    fa/'Sample_2 (paired) trimmed (paired) assembly.fa' \
    fa/'Sample_3 (paired) trimmed (paired) assembly.fa' \
    fa/'Sample_4 (paired) trimmed (paired) assembly.fa' \
    fa/'Sample_5 (paired) trimmed (paired) assembly.fa' > query5.fa
    
### Copy query sequences to $HOME/queries folder
cp query* $HOME/queries/.

## Step 2. Display BLAST databases on the GCP
docker run --rm ncbi/blast update_blastdb.pl --showall pretty --source gcp

## Download nt_v5 (nucleotide collection version 5) database
## This step takes approximately 10 min.  The following command runs in the background.
docker run --rm \
  -v $HOME/blastdb:/blast/blastdb:rw \
  -w /blast/blastdb \
  ncbi/blast \
  update_blastdb.pl --source gcp nt_v5 &

## At this point, confirm query/database have been properly provisioned before proceeding

## Check the size of the directory containing the BLAST database
## nt_v5 should be around 68 GB
du -sk $HOME/blastdb

## Check for queries, there should be three files - query.fa, query1.fa and query5.fa
ls -al $HOME/queries

## From this point forward, it may be easier if you run these steps in a script. 
## Simply copy and paste all the commands below into a file named script.sh
## Then run the script in the background `nohup bash script.sh > script.out &`

## Step 3. Run BLAST
## Run BLAST using query1.fa (Sample 1) 
## This command will take approximately 9 minutes to complete.
## Expected output size: 3.1 GB  
docker run --rm \
  -v $HOME/blastdb:/blast/blastdb:ro -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
  -v $HOME/queries:/blast/queries:ro \
  -v $HOME/results:/blast/results:rw \
  ncbi/blast \
  blastn -query /blast/queries/query1.fa -db nt_v5 -num_threads 16 \
  -out /blast/results/blastn.query1.denovo16s.out

## Run BLAST using query5.fa (Samples 1-5) 
## This command will take approximately 30 minutes to complete.
## Expected output size: 10.4 GB  
docker run --rm \
  -v $HOME/blastdb:/blast/blastdb:ro -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
  -v $HOME/queries:/blast/queries:ro \
  -v $HOME/results:/blast/results:rw \
  ncbi/blast \
  blastn -query /blast/queries/query5.fa -db nt_v5 -num_threads 16 \
  -out /blast/results/blastn.query5.denovo16s.out

## Run BLAST using query.fa (All 28 samples) 
## This command will take approximately 147 minutes to complete.
## Expected output size: 47.8 GB  
docker run --rm \
  -v $HOME/blastdb:/blast/blastdb:ro -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
  -v $HOME/queries:/blast/queries:ro \
  -v $HOME/results:/blast/results:rw \
  ncbi/blast \
  blastn -query /blast/queries/query.fa -db nt_v5 -num_threads 16 \
  -out /blast/results/blastn.query.denovo16s.out

## Stdout and stderr will be in script.out
## BLAST output will be in $HOME/results
``` 

This marks the end of our tutorial. If you do not need our data for further analysis, please [delete](https://cloud.google.com/compute/docs/instances/deleting-instance) the VM to prevent incurring additional cost. To do so, follow instructions in the section [Stop the GCP instance.](#stop-the-gcp-instance)

# Additional Resources
* BLAST:
    * [BLAST Command Line Applications User Manual](https://www.ncbi.nlm.nih.gov/books/NBK279696/)  
    * [BLAST Knowledge Base](https://support.nlm.nih.gov/knowledgebase/category/?id=CAT-01239)
    * [2.9.0 Docker file](https://github.com/ncbi/docker/blob/master/blast/2.9.0/Dockerfile) and  [release notes](https://www.ncbi.nlm.nih.gov/books/NBK131777/#_Blast_ReleaseNotes_BLAST_2_9_0_April_01)
    * [2.8.1 Docker file](https://github.com/ncbi/docker/blob/master/blast/2.8.1/Dockerfile) and  [release notes](https://www.ncbi.nlm.nih.gov/books/NBK131777/#_Blast_ReleaseNotes_BLAST_2_8_1_DECEMBER_1_)
    * [2.8.0 Docker file](https://github.com/ncbi/docker/blob/master/blast/2.8.0/Dockerfile) and [release notes](https://www.ncbi.nlm.nih.gov/books/NBK131777/#_Blast_ReleaseNotes_BLAST_2_8_0_March_28_)
    * [2.7.1 Docker file](https://github.com/ncbi/docker/blob/master/blast/2.7.1/Dockerfile) and [release notes](https://www.ncbi.nlm.nih.gov/books/NBK131777/#_Blast_ReleaseNotes_BLAST_2_7_1_October_2_)
* Docker: 
    * [Docker Community Forums](https://forums.docker.com)
    * [Docker Community Slack](https://blog.docker.com/2016/11/introducing-docker-community-directory-docker-community-slack/)
    * [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=docker+blast)
* Other:
    * [Common Workflow Language (CWL)](https://www.commonwl.org/) is a specification to describe tools and workflows.  This [GitHub Repository](https://github.com/ncbi/cwl-demos/tree/master/blast-pipelines) contains sample CWL workflows using containerized BLAST+.
    * [Google Cloud Platform](https://cloud.google.com/)
    * [NIH/STRIDES](https://datascience.nih.gov/strides)
    * [GitHub](https://github.com/ncbi)
    
or [email us.](mailto:blast-help@ncbi.nlm.nih.gov)

# Maintainer

[National Center for Biotechnology Information (NCBI)](https://www.ncbi.nlm.nih.gov/)  
[National Library of Medicine (NLM)](https://www.nlm.nih.gov/)  
[National Institutes of Health (NIH)](https://www.nih.gov/)

# License

Refer to the [license](https://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/scripts/projects/blast/LICENSE) and [copyright](http://ncbi.github.io/blast-cloud/dev/copyright.html) information for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses. As with any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

# Appendix
## Appendix A. Cloud and Docker Concepts

![Cloud Docker Simple](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/cloud-docker-simple.png "Cloud docker simple")

Figure 1. Docker and Cloud Computing Concept. Users can access compute resources provided by cloud service providers (CSPs), such as the Google Cloud Platform, using SSH tunneling (1). When you create a VM (2), a hard disk (also called a boot/persistent disk) (3) is attached to that VM. With the right permissions, VMs can also access other storage buckets (4) or other data repositories in the public domain. Once inside a VM with Docker installed, you can run a Docker image (5), such as NCBI's BLAST image. An image can be used to create multiple running instances or containers (6). Each container is in an isolated environment. In order to make data accessible inside the container, you need to use Docker bind mounts (7) described in this tutorial. 

*A Docker image can be used to create a Singularity image.  Please refer to Singularity's [documentation](https://www.sylabs.io/singularity/) for more detail.*

## Appendix B. Alternative Ways to Run Docker
  
As an alternative to what is described above, you can also run BLAST interactively inside a container.   


### Run BLAST+ Docker image interactively  
__When to use__: This is useful for running a few (e.g., fewer than 5-10) BLAST searches on small BLAST databases where you expect the search to complete in seconds/minutes.  
  
```
docker run --rm -it \
    -v $HOME/blastdb:/blast/blastdb:ro -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
    -v $HOME/queries:/blast/queries:ro \
    -v $HOME/results:/blast/results:rw \
    ncbi/blast \
    /bin/bash

# Once you are inside the container (note the root prompt), run the following BLAST commands.
blastp -query /blast/queries/P01349.fsa -db nurse-shark-proteins \
    -out /blast/results/blastp.out

# To view output, run the following command
more /blast/results/blastp.out

# Leave container
exit

```

In addition, you can run BLAST in [detached mode](https://docs.docker.com/engine/reference/run/#detached--d) by running a container in the background.  

### Run BLAST+ Docker image in detached mode 

__When to use__: This is a more practical approach if you have many (e.g., 10 or
more) BLAST searches to run or you expect the search to take a long time to execute. In this case it may be better to start the BLAST container in detached mode and execute commands on it. 
  
NOTE: Be sure to mount _all_ required directories, as these need to be
specified when the container is started.

```
# Start a container named 'blast' in detached mode
docker run --rm -dit --name blast \
    -v $HOME/blastdb:/blast/blastdb:ro -v $HOME/blastdb_custom:/blast/blastdb_custom:ro \
    -v $HOME/queries:/blast/queries:ro \
    -v $HOME/results:/blast/results:rw \
    ncbi/blast \
    sleep infinity

# Check the container is running in the background
docker ps -a
docker ps --filter "status=running"
```
Once the container is confirmed to be [running in detached mode](https://docs.docker.com/engine/reference/commandline/ps/), run the following BLAST command.
  
```
docker exec blast blastp -query /blast/queries/P01349.fsa \
    -db nurse-shark-proteins -out /blast/results/blastp.out

# View output
more $HOME/results/blastp.out

# stop the container
docker stop blast
```
If you run into issues with `docker stop blast` command, reset the VM from the GCP Console or restart the SSH session.  

## Appendix C. Transfer Files to/from a GCP VM

To copy the file `$HOME/script.out` in the home directory on a local machine to the home directory on a GCP VM named `instance-1` in project `My First Project` using GCP Cloud SDK.

GCP [documentation](https://cloud.google.com/compute/docs/instances/transfer-files)

First, install GCP [Cloud SDK]( https://cloud.google.com/sdk/) command line tools for your operating system.

```
# First, set up gcloud tools
# From local machine's terminal

gcloud init

# Enter a configuration name
# Select the sign-in email account
# Select a project, for example “my-first-project”
# Select a compute engine zone, for example, “us-east4-c”

# To copy the file $HOME/script.out to the home directory of GCP instance-1 
# Instance name can be found in your Google Cloud Console -> Compute Engine -> VM instances

gcloud compute scp $HOME/script.out instance-1:~

# Optional - to transfer the file from the GCP instance to a local machine's home directory

gcloud compute scp instance-1:~/script.out $HOME/.
```
