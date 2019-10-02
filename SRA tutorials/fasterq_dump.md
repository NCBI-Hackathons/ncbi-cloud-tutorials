# FASTERQ-DUMP

Fasterq-dump is an updated version of the fastq-dump program.  It has a significant speed upgrade compared to fastq-dump by using temporary files during output.

## Usage:
  fasterq-dump <path> [options]

## Options:

Option (short) | Option (long) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Description
---|---|---
  -o|--outfile |                    output-file
  -O|--outdir  |                    output-dir 
  -b|--bufsize |                    size of file-buffer default=1MB 
  -c|--curcache |                   size of cursor-cache default=10MB 
  -m|--mem   |                      memory limit for sorting default=100MB 
  -t|--temp   |                     where to put temp. files default=curr dir 
  -e|--threads |                    how many thread dflt=6 
  -p|--progress |                   show progress 
  -x|--details   |                  print details 
  -s|--split-spot |                 split spots into reads 
  -S|--split-files |                write reads into different files 
  -3|--split-3      |               writes single reads in special file 
&nbsp; |  --concatenate-reads|              writes whole spots into one file 
  -Z|--stdout |             print output to stdout   
  -f|--force |            force to overwrite existing file(s) 
  -N|--rowid-as-name |           use row-id as name 
&nbsp; |  --skip-technical |          skip technical reads 
&nbsp; |  --include-technical |         include technical reads 
  -P|--print-read-nr |         print read-numbers 
  -M|--min-read-len |        filter by sequence-len 
 &nbsp; | --table |       which seq-table to use in case of pacbio 
 &nbsp; | --strict                   |      terminate on invalid read 
  -B|--bases |     filter by bases 
  -A|--append |    append to output-file 
  -h|--help |   Output brief explanation for the program. 
  -V|--version |  Display the version of the program then quit. 
  -L|--log-level <level> | Logging level as number or enum string. One of (fatal\|sys\|int\|err\|warn\|info\|debug) or (0-6) Current default is warn 
  -v|--verbose |Increase the verbosity of the program status messages. Use multiple times for more verbosity. Negates quiet. 
  -q|--quiet |     Turn off all status messages for the program. Negated by verbose. 
&nbsp; |  --option-file <file> |    Read more options and parameters from the file. 

2.10.0 
