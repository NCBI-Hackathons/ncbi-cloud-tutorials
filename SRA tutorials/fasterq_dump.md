#FASTERQ-DUMP

Usage:
  fasterq-dump <path> [options]

Options:
  -o|--outfile                     output-file 
  -O|--outdir                      output-dir 
  -b|--bufsize                     size of file-buffer dflt=1MB 
  -c|--curcache                    size of cursor-cache dflt=10MB 
  -m|--mem                         memory limit for sorting dflt=100MB 
  -t|--temp                        where to put temp. files dflt=curr dir 
  -e|--threads                     how many thread dflt=6 
  -p|--progress                    show progress 
  -x|--details                     print details 
  -s|--split-spot                  split spots into reads 
  -S|--split-files                 write reads into different files 
  -3|--split-3                     writes single reads in special file 
  --concatenate-reads              writes whole spots into one file 
  -Z|--stdout                      print output to stdout 
  -f|--force                       force to overwrite existing file(s) 
  -N|--rowid-as-name               use row-id as name 
  --skip-technical                 skip technical reads 
  --include-technical              include technical reads 
  -P|--print-read-nr               print read-numbers 
  -M|--min-read-len                filter by sequence-len 
  --table                          which seq-table to use in case of pacbio 
  --strict                         terminate on invalid read 
  -B|--bases                       filter by bases 
  -A|--append                      append to output-file 
  -h|--help                        Output brief explanation for the program. 
  -V|--version                     Display the version of the program then 
                                   quit. 
  -L|--log-level <level>           Logging level as number or enum string. One 
                                   of (fatal|sys|int|err|warn|info|debug) or 
                                   (0-6) Current/default is warn 
  -v|--verbose                     Increase the verbosity of the program 
                                   status messages. Use multiple times for more 
                                   verbosity. Negates quiet. 
  -q|--quiet                       Turn off all status messages for the 
                                   program. Negated by verbose. 
  --option-file <file>             Read more options and parameters from the 
                                   file. 

/usr/local/ncbi/sra-tools/bin/fasterq-dump : 2.10.0 ( 2.10.0-a )
