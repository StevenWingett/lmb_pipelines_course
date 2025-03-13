# Answers

## Next-generation sequencing
### Exercise 1
#### a
1.    `zcat sample1.fastq.gz | more`

2.    `ls -l *.fastq.gz`

We agree

3.    
`md5sum *.gz`

We agree

4.    
This is a one-line command:

`zcat sample* | gzip > combined.fastq.gz`

Better compression than cat *.gz

Beware that zcat *.fastq.gz | gzip > combined.fastq.gz will not work since combined.fastq.gz will be considered an input file!

1.    
`zcat combined.fastq.gz | wc -l`

`1200 / 4 = 300`

#### b
3.    
`fastqc comined.fastq.gz`

Then copy to local machine

#### c
1.    Jibberish

2.    `samtools view -h mapping_results.bam`

3.    1257


## Nextflow and nf-core
### Exercise 2
#### b
Get from GUIde piper:

`nextflow run nf-core/fetchngs -r 1.7 --input to_download.txt -config /public/singularity/containers/nextflow/lmb-nextflow/lmb.config -queue-size 4 --outdir results -bg`

#### c
`wget -L https://raw.githubusercontent.com/nf-core/rnaseq/master/bin/fastq_dir_to_samplesheet.py`

`python3 ./fastq_dir_to_samplesheet.py -r1 _1.fastq.gz -r2 _2.fastq.gz FASTQ/ samplesheet.csv`

`nextflow run nf-core/rnaseq -r 3.8.1 --input samplesheet.csv --genome saccharomyces_cerevisiae.R64-1-1.release_105 -config /public/singularity/containers/nextflow/lmb-nextflow/lmb.config --outdir results --deseq2_vst -bg`
