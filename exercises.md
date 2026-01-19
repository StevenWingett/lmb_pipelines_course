![LMB Logo](assets/lmb_logo.png)
 
 <hr> 

Licence
This manual is © 2026, Steven Wingett

This manual is distributed under the creative commons Attribution-Non-Commercial-Share Alike 2.0 licence. This means that you are free:

•	to copy, distribute, display, and perform the work

•	to make derivative works


Under the following conditions:

•	Attribution. You must give the original author credit.

•	Non-Commercial. You may not use this work for commercial purposes.

•	Share Alike. If you alter, transform, or build upon this work, you may distribute the resulting work only under a licence identical to this one.

Please note that:

•	For any reuse or distribution, you must make clear to others the licence terms of this work.
•	Any of these conditions can be waived if you get permission from the copyright holder.
•	Nothing in this license impairs or restricts the author's moral rights.

Full details of this licence can be found at 
http://creativecommons.org/licenses/by-nc-sa/2.0/uk/legalcode

<hr>

# Next-generation sequencing
## Exercise 1
### a.
You have received 3 FASTQ files named sample1.fastq.gz, sample2.fastq.gz and sample3.fastq.gz.

1.  View the first few lines of one of the files to confirm it is indeed a FASTQ file.

2.  Let’s check the file transfer was successful.  The file sizes (bytes) should be:

    `sample1.fastq.gz` : 3444

    `sample2.fastq.gz` : 3477
    
    `sample3.fastq.gz` : 3761

Is this what you see?

3. Installed on the cluster is a program named md5sum that produces a string of letters, numbers and symbols to represent a file.  Even a small change in a file will cause different characters to be returned.  This is an excellent way to check files are identical before and after transfer.  The md5 checksums of the original files are:

        sample1.fastq.gz : 58b3dcc9e3bc866b406aa58c3d4b07dd  
        sample2.fastq.gz : d5849a41e5661e853c6d5e309e558a03  
        sample3.fastq.gz : c563d467ee22582728d02280826fd15e

Do you agree with these results?

4. Using command line Linux, combine the 3 FASTQ files into a gzipped single file named `combined.fastq.gz`.

5. How many reads are there in your newly created FASTQ file?  Use the command line to ascertain this (the command `wc` may help you).

### b. 
We are now going to try out a couple of bioinformatics tools.  However, since they are not distributed with Linux as standard, your system will need to be told where to find them.  The executable files are kept in: `/public/genomics/soft/bin/`

1.  Let’s now temporarily add that location to our $PATH variable.  

        export PATH="$PATH:/public/genomics/soft/bin/"

Confirm that this new folder has been added to your path.

2.  FastQC is software used by most labs to assess the overall QC of their data and we want to run it on our combined FASTQ file.  Let’s read the manual:

        man fastqc

Ah!  Since FastQC is external software, help will not be provided in the Linux built-in manual.  However, most external software tools will provide help with one of the following commands:

        [software_name] -help 
        [software_name] --help

Try this for FastQC.

3.  Analyse your combined dataset `combined.fastq.gz` with FastQC.  When it has finished running, copy the resulting HTML file to your local machine and see if you can interpret the results (please note that this is a tiny dataset for demonstration purposes).

### c.
1. Try viewing the file `mapping_results.bam` with `cat`.  What do you see?  Does `zcat` help?

2. Read the help for `samtools` to learn how to read the BAM file.  Browse the file.  Can you see the headers?  Can you see the aligned reads?

3. Run `samtools flagstat` on the file.  How many mapped reads are there?

<hr>

# Nextflow and nf-core
## Exercise 2
### a.
1. Let’s set up Nextflow on your system. Go to the head node and execute the following one-line command:

        $ curl -s https://raw.githubusercontent.com/StevenWingett/lmb-nextflow/main/nextflow_setup_cluster.sh | bash

From what you have learnt in the course already, does this command make sense to you?  This command downloads a bash script from a remote server and then runs it.

2. There is a hidden Bash configuration file in your home directory named `.bashrc`.  Running the command above should have edited this file?  Has this happened (check the date).

(Remember that hidden files begin with a dot and can be viewed with `ls --all`).

3. Read the contents of the `.bashrc` file.  Changes will have been made to the end of the file.  Can you make sense of these?

4. Run the testing command:

        nextflow run hello

What do you see? 

### b. 
1. Let’s use nf-core to download FASTQ files.  Navigate to the webpage: https://www.ncbi.nlm.nih.gov/sra/SRR5413016

We would like to download the FASTQ files for the accessions `SRR5413015` and `SRR5413016`.  

Create a text file with each of these accessions listed on a separate line (make sure there are no spaces or blank lines in this file).  Name the file `to_download.txt`.

Now use GUIde Piper to create a Nextflow command to download these files.  

Go to the internal GUIde Piper website (http://guidepiper) and select the “Download Data” option.  Input the name of your file (`to_download.txt`) and use GUIde Piper to generate a Nextflow command.  Now run this command on a head node.

2. Did the job complete successfully?  Did you receive an email?  Can you see the downloaded FASTQ files (look inside any folders generated)?

3. Can you see the metadata?  Copy it to your local machine (e.g. use FileZilla) and view in suitable software (e.g. Excel)?

4. Let’s make some nicer filenames.  Download this file:
   https://raw.githubusercontent.com/StevenWingett/lmb-nextflow/main/ancillary_scripts/fetchngs_renamer.py

Go to the folder that contains the results folder and then run the command:

        python3 fetchngs_renamer.py

What did this do?

### c.
Suppose you have received the results of a paired-end, non-strand specific *Saccharomyces cerevisiae* RNA-seq experiment.  There are 2 conditions, namely A and B.  The relevant files are:

        yeast_rna_seq_A_1.fastq.gz
        yeast_rna_seq_A_2.fastq.gz
        yeast_rna_seq_B_1.fastq.gz
        yeast_rna_seq_B_2.fastq.gz

These are actually “toy” datasets which contain only a fraction of the original data in order to enable processing within the timeframe of the course.  If you have not been given these files by the course instructor, you can create them yourself - see footnote and the end of this document.


Process these datasets with the appropriate nf-core pipeline and settings.  To do this, read the nf-core documentation about the RNA-seq pipeline.  Then create a command using GUIde-Piper to process the RNA-seq datasets.

### d.
Let’s look at the results of the processing.  To start with, open the file `muliqc_report.html` that summarised the results of the pipeline.

1. What are the percentage of uniquely mapped reads for the Condition A and Condition B samples when using the aligner STAR?

2. Are the bulk of the aligned reads mapping to exonic, intronic or intergenic regions?

3. We specified that this was a non-strand specific library.  Were we correct in doing that?

4. What was the most frequent %GC for the reads in the input files?

5. Approximately what percentage of the reads terminated with adapter sequence?

6. What is the most frequent inner distance / insert size between the reads?  What does this mean?

7. It’s time to write up our Materials and Methods for a paper!  What version of STAR did we use to perform the alignment?

### e.
Let’s look at some more of the files we have produced.

1. Open the execution report.  Review the compute cluster resources used by the pipeline.  This is a good way to learn which tasks are more computationally intensive.

2. Can you find the BAM files?  Are you able to browse them using the command line?

3. View the file `salmon.merged.gene_tpm.tsv`.  Can you work out what is contained in here?

<hr>

### Footnote - making Sample A/B files

Sample A and B refer to accessions SRR3567555 and SRR3567679 respectively.  Download them using the nf-core pipeline used previously.  Then, subset the files by extracting the first 100,000 reads from each.  I recommend using `zcat` in conjunction with `head` to do this:*

        zcat SRX1790985_SRR3567555_1.fastq.gz | head -400000 | gzip > yeast_rna_seq_A_1.fastq.gz
        zcat SRX1790985_SRR3567555_2.fastq.gz | head -400000 | gzip > yeast_rna_seq_A_2.fastq.gz
        zcat SRX1791046_SRR3567679_1.fastq.gz | head -400000 | gzip > yeast_rna_seq_B_1.fastq.gz
        zcat SRX1791046_SRR3567679_2.fastq.gz | head -400000 | gzip > yeast_rna_seq_B_2.fastq.gz

Typically I would not recommend subsetting a FASTQ file by extracting reads only from the start of the file since this may introduce biases, but for testing purposes this approach is fine.
