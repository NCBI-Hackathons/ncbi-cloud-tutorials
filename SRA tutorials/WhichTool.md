# Tool Suggestions for Common Actions

Knowing which tool is best for your planned process is important and can save you both time and expense.  
This guide will suggest the program to use for some common use cases.

Goal | Recommended Tool | Comments
--- | --- | ---
Download a copy of source or SRR format data to my own storage | prefetch | Use `prefetch -T any <accession>` to include the source and analysis files as well.
Read Files without storing locally | fusera | In most cases fusera will be the best choice for read in place operations.
See a list of all files available for an accession | fusera | The directory listing view from fusera is an easy way to view data you may choose to download a copy of to your own storage.
Get fastq format data as one or more files | fasterq-dump | This program outputs fastq format data more quickly than fastq-dump but does require a bit of extra space for temporary files.
Get bam/sam format data in a consistent format | sam-dump | sam-dump will output data that as gone through the ETL process at NCBI to make it as consistent as possible across submitters.
Get data exactly as it was submitted | prefetch/fusera | Accession the source files can be done using either prefetch to store locally or fusera to access remotely.

The primary goal of fusera is to provide access to sequencing data that resides in storage that is not local but make it appear local.  This solves issues with providing URLs to tools that expect file names or pointers.

The SRA Toolkit can be used to either download the SRA format runs or the additional analysis or source files for a run when those are available.  If local file-caching has been enabled in the toolkit using the toolkit programs that dump data (fastq-dump, sam-dump, etc) will also store the data that was downloaded as a file with the name format <accession>.sra.cache.  If the entire run was downloaded in the process of using the dumper, the file will instead just be the accession and will be the complete file for the run.
  
  
