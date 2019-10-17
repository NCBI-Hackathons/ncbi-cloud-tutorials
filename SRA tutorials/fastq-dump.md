# Fastq-dump

## Usage:
**/usr/local/ncbi/sra-tools/bin/fastq-dump [options] \<path> [\<path>...]**

**/usr/local/ncbi/sra-tools/bin/fastq-dump [options] \<accession>**  

## INPUT
Option (short) | Option (long) | Description
---|---|---
-A|--accession <accession> | Replaces accession derived from <path> in filename(s) and deflines (only for single table dump) 
&nbsp;|--table <table-name> |            Table name within cSRA object, default is "SEQUENCE" 

## PROCESSING
### Read Splitting
Sequence data may be used in raw form or split into individual reads

Option (short) | Option (long) | Description
---|---|---
&nbsp;|--split-spot |                    Split spots into individual reads 


### Full Spot Filters                  
Applied to the full spot independently of --split-spot

Option (short) | Option (long) | Description
---|---|---
-N|--minSpotId <rowid> |          Minimum spot id 
-X|--maxSpotId <rowid> |          Maximum spot id 
&nbsp;|--spot-groups <[list]> |          Filter by SPOT_GROUP (member): name[,...] 
-W|--clip |                       Remove adapter sequences from reads 

### Common Filters                     
Applied to spots when --split-spot is not set, otherwise - to individual reads

Option (short) | Option (long) | Description
---|---|---
-M|--minReadLen <len> |           Filter by sequence length >= \<len\> 
-R|--read-filter <[filter]> &nbsp; &nbsp; |     Split into files by READ_FILTER value optionally filter by value: pass\|reject\|criteria\|redacted 
-E|--qual-filter |                Filter used in early 1000 Genomes data: no sequences starting or ending with >= 10N 
&nbsp;|--qual-filter-1 |                 Filter used in current 1000 Genomes data 

### Filters based on alignments        
Filters are active when alignment data are present

Option (short) | Option (long) | Description
---|---|---
&nbsp;|--aligned |                       Dump only aligned sequences 
&nbsp;|--unaligned |                     Dump only unaligned sequences 
&nbsp;|--aligned-region <name[:from-to]> | Filter by position on genome. Name can either be accession.version (ex: NC_000001.10) or file specific name (ex: "chr1" or "1"). "from" and "to" are 1-based coordinates 
&nbsp;|--matepair-distance <from-to\|unknown> &nbsp; &nbsp; | Filter by distance between matepairs. Use "unknown" to find matepairs split between the references. Use from-to to limit matepair distance on the same reference 

### Filters for individual reads       
Applied only with --split-spot set

Option (short) | Option (long) | Description
---|---|---
&nbsp;|--skip-technical |                Dump only biological reads 

## OUTPUT

Option (short) | Option (long) | Description
---|---|---
-O|--outdir <path> |              Output directory, default is working directory '.' ) 
-Z|--stdout |                     Output to stdout, all split data become joined into single stream 
&nbsp;|--gzip |                          Compress output using gzip: deprecated, not recommended 
&nbsp;|--bzip2 |                         Compress output using bzip2: deprecated, not recommended 

## Multiple File Options              
Setting these options will produce more than 1 file, each of which will be suffixed according to splitting criteria.

Option (short) | Option (long) | Description
---|---|---
&nbsp;|--split-files |                   Write reads into separate files. Read number will be suffixed to the file name. NOTE! The `--split-3` option is recommended. In cases where not all spots have the same number of reads, this option will produce files that WILL CAUSE ERRORS in most programs which process split pair fastq files. 
&nbsp;|--split-3 |                       3-way splitting for mate-pairs. For each spot, if there are two biological reads satisfying filter conditions, the first is placed in the `*_1.fastq` file, and the second is placed in the `*_2.fastq` file. If there is only one biological read satisfying the filter conditions, it is placed in the `*.fastq` file.All other reads in the spot are ignored. 
-G|--spot-group |                 Split into files by SPOT_GROUP (member name) 
-R|--read-filter <[filter]> &nbsp; &nbsp; &nbsp; |    Split into files by READ_FILTER value optionally filter by value: pass\|reject\|criteria\|redacted 
-T|--group-in-dirs |              Split into subdirectories instead of files 
-K|--keep-empty-files |           Do not delete empty files 

## FORMATTING
### Sequence

Option (short) | Option (long) | Description
---|---|---
-C|--dumpcs <[cskey]> |           Formats sequence using color space (default for SOLiD),"cskey" may be specified for translation 
-B|--dumpbase |                   Formats sequence using base space (default for other than SOLiD). 

### Quality

Option (short) | Option (long) | Description
---|---|---
-Q|--offset <integer> |           Offset to use for quality conversion, default is 33 
&nbsp;|--fasta <[line width]> |          FASTA only, no qualities, optional line wrap width (set to zero for no wrapping) 
&nbsp;|--suppress-qual-for-cskey |       suppress quality-value for cskey 

### Defline

Option (short) | Option (long) | Description
---|---|---
-F|--origfmt |                    Defline contains only original sequence name 
-I|--readids |                    Append read id after spot id as 'accession.spot.readid' on defline 
&nbsp;|--helicos |                       Helicos style defline 
&nbsp;|--defline-seq <fmt> |             Defline format specification for sequence. 
&nbsp;|--defline-qual <fmt> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |            Defline format specification for quality.  <fmt> is string of characters and/or variables. The variables can be one of: $ac - accession, $si spot id, $sn spot name, $sg spot group (barcode), $sl spot length in bases, $ri read number, $rn read name, $rl read length in bases. '[]' could be used for an optional output: if all vars in [] yield empty values whole group is not printed. Empty value is empty string or for numeric variables. Ex: @$sn[_$rn]/$ri '_$rn' is omitted if name is empty
 
### OTHER:

Option (short) | Option (long) | Description
---|---|---
&nbsp;|--disable-multithreading &nbsp; &nbsp; &nbsp; |        disable multithreading 
-h|--help |                       Output brief explanation of program usage 
-V|--version |                    Display the version of the program 
-L|--log-level <level> |          Logging level as number or enum string One of (fatal|sys|int|err|warn|info) or (0-5) Current/default is warn 
-v|--verbose |                    Increase the verbosity level of the program Use multiple times for more verbosity 
&nbsp;|--ncbi_error_report |             Control program execution environment report generation (if implemented). One of (never\|error\|always). Default is error 
&nbsp;|--legacy-report |                 use legacy style 'Written spots' for tool 

2.10.0
