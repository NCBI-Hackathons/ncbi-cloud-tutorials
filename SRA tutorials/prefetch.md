# Prefetch

## Usage:
###  prefetch [options] \<SRA accession | kart file> [...]  
  Download SRA or dbGaP files and their dependencies

###  prefetch [options] \<URL> --output-file <FILE>  
  Download URL to FILE

###  prefetch [options] \<URL> [...] --output-directory <DIRECTORY>  
  Download URL or URL-s to DIRECTORY

###  prefetch [options] \<SRA file> [...]  
  Check SRA file for missed dependencies and download them

###  prefetch --list \<kart file> [...]  
  List content of kart file


## Options:
Option (short) | Option (long) | Description
---|---|---
-T|--type |                        Specify file type to download. Default: sra 
-t|--transport <value> |          Transport: one of: fasp; http; both. (fasp only; http only; first try fasp (ascp), use http if cannot download using fasp). Default: both 
&nbsp; | --location |              Location of data 
-N|--min-size <size> |             Minimum file size to download in KB (inclusive). 
-X|--max-size <size> |            Maximum file size to download in KB (exclusive). Default: 20G 
-f|--force <value>   |            Force object download one of: no, yes, all. no [default]: skip download if the object if found and complete; yes: download it even if it is found and is complete; all: ignore lock files (stale locks or it is being downloaded by another process: use at your own risk!) 
-p|--progress <value> |           Time period in minutes to display download progress (0: no progress), default: 1 
&nbsp;|--eliminate-quals |               Don't download QUALITY column 
-c|--check-all |                  Double-check all refseqs 
-l|--list |                       List the content of kart file 
-n|--numbered-list |              List the content of kart file with kart row numbers 
-s|--list-sizes |                 List the content of kart file with target file sizes 
-R|--rows <rows> |                Kart rows to download (default all). row list should be ordered 
-o|--order <value> |              Kart prefetch order when downloading kart: one of: kart, size. (in kart order, by file size: smallest first), default: size 
-a|--ascp-path <ascp-binary\|private-key-file> | Path to ascp program and private key file (asperaweb_id_dsa.putty) 
&nbsp;| --ascp-options <value> |          Arbitrary options to pass to ascp command line 
-o|--output-file <FILE>  |        Write file to FILE when downloading single file 
-O|--output-directory <DIRECTORY> | Save files to DIRECTORY/ 
-h|--help |                       Output brief explanation for the program. 
-V|--version |                    Display the version of the program then quit. 
-L|--log-level <level> |          Logging level as number or enum string. One of (fatal\|sys\|int\|err\|warn\|info\|debug) or (0-6) Current/default is warn 
-v|--verbose  |                   Increase the verbosity of the program status messages. Use multiple times for more verbosity. Negates quiet. 
-q|--quiet |                       Turn off all status messages for the program. Negated by verbose. 
&nbsp;| --option-file <file> |             Read more options and parameters from the file. 

/usr/local/ncbi/sra-tools/bin/prefetch : 2.10.0 ( 2.10.0-a )
