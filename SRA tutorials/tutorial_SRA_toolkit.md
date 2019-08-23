# SRA Toolkit Cloud Installation Guide
Using SRA format data stored in the cloud will require the SRA Toolkit to be installed.  This guide will teach you how to install and configure the SRA Toolkit on a cloud computing environment using
- [Amazon EC2](#Install-the-Toolkit-in-Amazon-EC2)
- [Google GCP](#Install-the-Toolkit-in-Google-GCP)

## Install the Toolkit in Amazon EC2
Start a new EC2 compute instance.  Using the 'Amazon Linux 2 AMI 2.0.20190618 x86_64 HVM gp2' a t2.micro is the minimum instance capacity known to work with this software.
```
curl -O https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/cloud/setup-EC2.sh
sudo sh setup-EC2.sh
```
A large number of lines will scroll by as the packages needed for the toolkit installation are downloaded and configured.  Once the toolkit and depenancies have finished installing, the toolkit needs to be added to the user path. 

`source /etc/profile.d/sra-tools.sh`

Note: setup-EC2.sh uses yum to install the needed packages.  Other distributions of linux could be used but additional configurations may be necessary for the script used in this example.

## Install the Toolkit in Google GCP
Start a new GCP compute instance.  The g1-small class is known to work.  NOTE: The f1-micro class is not sufficient to install the toolkit.

```
curl -O https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/cloud/setup-GCP.sh
sudo sh setup-GCP.sh
```

A large number of lines will scroll by as the packages needed for the toolkit installation are downloaded and configured.  Once the toolkit and depenancies have finished installing, the toolkit needs to be added to the user path. 

`source /etc/profile.d/sra-tools.sh`

Note: setup-GCP.sh uses apt-get and may work or need to be modified for various Linux using Debian package management. 

## Configure the SRA Toolkit
The Toolkit comes with a command line or GUI configuration program.  You can read and write data to the cloud account without running the configuration program, however without configuration the default output location is the current working directory and the toolkit will not be able to provide egress charge payment information to the cloud provider.

The vdb-config program will allow you to set the expected behavior of the SRA toolkit

`vdb-config -i` 

When vdb-config opens, it will start on the Main page. Make sure Enable Remote Access has an X in the box.  

![SRA Toolkit Configuration](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/tkt_main.png "SRA Toolkit Configuration Main Page")

## Cache
Set or update the cache directory by pressing `c` on the keyboard.  If you have an attached stable storage for work (AWS S3 or Google GS) you likely want to set the public user-repository to a directory in the bucket.

The process-local location is a cache that is used by individual toolkit processes and cleaned when the processes end.  This should be set as local storage for the compute instance.

## Cloud Provider Configuration

By pressing `a` on the keyboard for Amazon AWS or `g` on the keyboard for Google GCP, you can configure the usage for your cloud provider.  Press `e` to enable user pays options for accessing data that is not stored in a free to access location.  The current free data is stored at NCBI, in the AWS free bucket, or is stored in the cloud region that your client is currently running in.  The location of data in the cloud can be found using the Run Selector DATASTORE_region values.  The `r` report cloud instance identity option will allow the toolkit to report the identity of the cloud instance host to NCBI when requesting data.  This information is used to understand the location of the cloud compute instance to provide access to free data locations for that compute instance when possible.  

Additional information about configuring the SRA Toolkit can be found [here](https://github.com/ncbi/sra-tools/wiki/04.-Toolkit-Configuration).

## Testing the SRA Toolkit Installation
To test the installation of the toolkit, we will output a small bit of fastq to STDOUT.

`fastq-dump -X 5 -Z SRR1605529`

This will test that the SRR accession data as well as the genome reference repository stored at NCBI is accessible.

# Upload an Accession List to the Cloud Instance
Getting a list of accessions from your local computer to the cloud can be accomplished in a number of ways.  The process involves copying a list of accessions from either the SRA Run Selector, Entrez, or other method to the cloud computer instance where the accessions are needed.  For short lists this can be done by using copy and paste with a text editor like vim or nano on the cloud compute instance.  For longer lists a text file such as the SRR_Acc_List.txt file from Run Selector may need to be copied to the cloud instance.

## Using scp on a computer running Unix, OSX, or Windows to upload to AWS:

For this example we will use `scp` because it should be available on most or all computers you might use.  If you are using a Windows computer, you will need to open a Command Prompt window first.

`scp -i MyKeyPair.pem SRR_Acc_List.txt ec2-user@18.191.41.51:/home/ec2-user/`

- MyKeyPair.pem - The key pair that was selected when starting the AWS EC2 isntance.  A path 
- SRR_Acc_List.txt - The list of accessions to transfer to the cloud compute instance.
- ec2-user@18.191.41.51:/home/ec2-user/ - **MAKE SURE TO USE YOUR INSTANCE INFO RATHER THAN THIS EXAMPLE** The cloud compute instance the file is being transferred to. The format of this field is username@ip.address:/location/of/directory

