# KneadData #

----

 * Download the KneadData software ( [kneaddata.tar.gz](https://bitbucket.org/biobakery/kneaddata/downloads/kneaddata_v0.4.3.tar.gz) ) then follow the [steps to install and run](#markdown-header-getting-started-with-kneaddata).

 * If you use the KneadData software, please cite our manuscript: TBD

----

KneadData is a tool designed to perform quality control on metagenomic sequencing data, especially data from microbiome experiments. In these experiments, samples are typically taken from a host in hopes of learning something about the microbial community on the host. However, metagenomic sequencing data from such experiments will often contain a high ratio of host to bacterial reads. This tool aims to perform principled in silico separation of bacterial reads from these "contaminant" reads, be they from the host, from bacterial 16S sequences, or other user-defined sources.


## Getting Started with KneadData ##

### Requirements ###

1.  [Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) (version == 0.33) (automatically installed)
2.  [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) (version >= 2.1) (automatically installed)
3.  [Python](http://www.python.org/) (version >= 2.7)
4.  [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html)
5.  Operating system (Linux or Mac)

Optionally, [BMTagger](ftp://ftp.ncbi.nlm.nih.gov/pub/agarwala/bmtagger/) can be used instead of Bowtie2.

The executables for the required software packages should be installed in your $PATH. Alternatively, you can provide the location of the Bowtie2 install ($BOWTIE2_DIR) with the following KneadData option “--bowtie2-path $BOWTIE2_DIR”. 

### Installation ###

Before installing KneadData, please install the Java Runtime Environment (JRE). First [download](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) the JRE for your platform. Then follow the instructions for your platform: [Linux 64-bit](http://docs.oracle.com/javase/8/docs/technotes/guides/install/linux_jre.html#CFHIEGAA) or [Mac OS](http://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jre.html#jre_8u40_osx). At the end of the installation, add the location of the java executable to your $PATH.

1. Download and unpack the KneadData software
    * Download the software: [kneaddata.tar.gz](https://bitbucket.org/biobakery/kneaddata/downloads/kneaddata_v0.4.3.tar.gz)
    * `` $ tar zxvf kneaddata.tar.gz ``
    * `` $ cd kneaddata ``
2. From the KneadData directory, install KneadData
    * `` $ python setup.py install ``
    * This command will automatically install Trimmomatic and Bowtie2. To bypass the install of dependencies, add the option "--bypass-dependencies-install".
    * If you do not have write permissions to '/usr/lib/', then add the option "--user" to the install command. This will install the python package into subdirectories of '~/.local'. Please note when using the "--user" install option on some platforms, you might need to add '~/.local/bin/' to your $PATH as it might not be included by default. You will know if it needs to be added if you see the following message ``kneaddata: command not found`` when trying to run KneadData after installing with the "--user" option.
3. Download the reference database to $DIR
    * `` $ kneaddata_database --download human bowtie2 $DIR ``
    * When running this command, $DIR should be replaced with the full path to the directory you have selected to store the database.


### How to Run ###

#### Basic usage ####

`` $ kneaddata --infile1 $INPUT --reference-db $DATABASE ``

```
$INPUT = a single end fastq file
$DATABASE = the index of the KneadData database
```

For paired end reads, add the option “--infile2 $INPUT2” to provide the second set of reads. Also please note that more than one reference database can be provided.

Three types of output files will be created:

1. `` $INPUT_$DATABASE_clean.fastq ``
2. `` $INPUT_$DATABASE_contam.fastq ``
3. `` $INPUT_output.fastq ``

Files of type #1 and #2 will be created for each reference database provided.


#### Demo run ####

The examples folder contains a demo input file. This file is a single read, fastq format.

`` $ kneaddata --infile1 examples/demo.fastq --reference-db examples/demo_db ``


