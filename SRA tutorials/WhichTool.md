# How to Choose the Correct Tool to Use

Knowing which tool will fit your planned process is important and can save you both time and expense.  
This guide will explain what scenarios each tools are best suited for and when the SRA Toolkit or fusera will be needed.

Goal | Recommended Tool | Comments
--- | --- | ---
Download a copy of source or SRR format data to my own storage | prefetch | Use `prefetch -T any <accession>` to include the source and analysis files as well.
Read Files without storing locally | fusera | In most cases fusera will be the best choice for read in place operations
See a list of all files available for an accession | fusera | The directory listing view from fusera is an easy way to view data you may choose to download a copy of to your own storage.


The primary goal of fusera is to provide access to sequencing data that resides in storage that is not local but make it appear local.  This solves issues with providing URLs to tools that expect file names or pointers.

The SRA Toolkit can be used to either download the SRA format runs or the additional analysis or source files for a run when those are available.  If local file-caching has been enabled in the toolkit using the toolkit programs that dump data (fastq-dump, sam-dump, etc) will also store the data that was downloaded as a file with the name format <accession>.sra.cache.  If the entire run was downloaded in the process of using the dumper, the file will instead just be the accession and will be the complete file for the run.
  
  
