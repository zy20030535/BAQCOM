######################################################################
### CNPSA_Scripts - Quality Control (Trimmomatic) and mapping (STAR)
######################################################################


The CNPSA_Scripts is a friendly-user pipeline which implements two automated pipelines for RNA-Seq analysis using Trimmomatic for QC and  STAR for mapping the transcriptomes.


################
### INSTALATION
################


#STEP.1 - Install R and required libraries
	#Install 'optparse' and 'parallel' packages


#################
### CONFIGURATION
#################


#STEP.2 - Download this repository to a preference path:
	 $ git clone https://github.com/hanielcedraz/CNPSA_Scripts.git

#STEP.3 - Run install.sh. This file will replace the trimmomatic path into the script.R and update ~/bashrc directory path, so you can call the files from any directory.

	$ ./install.sh

#############
### RUNNING
#############

#STEP.4 - Create a directory named 00-Fastq and move the .fastq.gz files into this directory:
	$ mkdir 00-Fastq

#STEP.5 - Create samples.txt:

	$ create_samples.sh

#This script will work perfectly if the file names in the 00-Fastq directory follow the structure:

#	File R1: SAMPLEID_any_necessary_information_R1_001.fastq.gz
#	File R2: SAMPLEID_any_necessary_information_R2_001.fastq.gz

# If the files are splited in more than one R1 or R2 will be necessary to combine the equal R1 and R2 files. 
	#you may follow this command: gunzip -c raw_data/SAMPLE_*R1* > 00-Fastq/SAMPLEID_any_information_R1_001.fastq; gzip SAMPLEID_any_information_R1_001.fastq.gz gunzip -c raw_data/SAMPLE_*R2* > 00-Fastq/SAMPLEID_any_information_R2_001.fastq; gzip SAMPLEID_any_information_R2_001.fastq.gz
	

#STEP.6 - Run the quality control with Trimmomatic (qc_trimmomatic pipeline):

# -p is the number of processors
# -a is the name of adapter. Default=TruSeq2-PE.fa

	#Other options for adapters (-a):
		#NexteraPE-PE.fa
		#TruSeq2-SE.fa
		#TruSeq3-PE-2.fa
		#TruSeq3-PE.fa
		#TruSeq3-SE.fa

	$ qc_trimmomatic-V.0.0.1.R -p 36 

STEP.7 - Mapping with STAR (mapping_STAR pipeline):

#Generate the genome indexes files needs to be performed only one for each genome/annotation version.  After the index generation step, the mapping will be started automatically.

#This code will generate index and mapping.

> mapping_STAR-V-0.0.1.R -p 20 -m /genome_annotation_directory/annotation_version/ -t /genome_annotation_directory/genome.fa -g /enome_annotation_directory/annotation_version/annotation_version.gtf

#Run at second time

> mapping_STAR-V-0.0.1.R -o 20 -m /dir/release_version/

#obs. If needs to run the script with more than 20 thread, it must change ulimit in the system used (see "increasing_Limit_CentOS_7" file ==> https://naveensnayak.com/2015/09/17/increasing-file-descriptors-and-open-files-limit-centos-7/).

