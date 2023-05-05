## Genomics_lab
Lab Overview:
Identify human SRA kidney files from the SRA archive and create a GEM from them. 
Normalize the GEM In this lab, and merge it with preprocessed RNAseq data.

Lab Objective:
Identify human SRA kidney files from the SRA archive and create a GEM from them. 
Normalize the GEM In this lab
Create histogram of the GEM

Lab Tasks:

Step A. Install NCBI SRA Toolkit in Linux VM.

These instructions were taken from:

https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit

Step A. Download sra-toolkit.

wget http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz
Step B. Install sra-toolkit.

Unpack the tarball.

tar -vxzf FILE_NAME
Add the sratoolkit bin directory to your PATH

Edit your ~/.bashrc by adding the sra-toolkit ‘bin’ directory by editing the file in a text editor (e.g. nano, vi) or add the directory path from the command line and reloading the .bashrc file like this:

#e.g.: echo 'export PATH=/home/student/software/sratoolkit.3.0.0-ubuntu64/bin/:$PATH' >> ~/.bashrc

source ~/.bashrc
Note that you may have downloaded the software into a different directory path. If different than above, use the actual path that leads to the bin directory in the export command. Reload your .bashrc environment to include the path to the commands with this command.

Step C.  Configure sra-toolkit to download data from NCBI with this sra-tkoolit command:

vdb-config -i
It should configure correctly if you just run the command and save the configuration. Test the connection using this command:

prefetch SRR5139394
prefetch SRR5139396
prefetch SRR5139398

#this is RNA-Seq,300,12606083400,PRJNA359795,SAMN06198300,5508278188,GEO,public,"fastq,sra",s3,s3.us-east-1,SRX2458154,Illumina HiSeq 4000,PAIRED,cDNA,TRANSCRIPTOMIC,Homo sapiens,ILLUMINA,2018-01-08T00:00:00Z,2017-01-03T13:18:00Z,1,GSM2443359,SRP095950,A,female,GSM2443359,Kidney,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,

fastq-dump --stdout SRR390728 | head -n 8
This will download the SRA file in .sra format in a new folder. fastq-dump and fasterq-dump convert the human unreadable SRA file into the human readable FASTQ format file filled with the DNA sequences and base quality scores.

Note: If the sequences are paired, that mean both ends of the DNA was sequenced from the same molecule so you get 2 FASTQ format files for that sample, and they need to be co-mapped to the reference genome to maxmize DNA-DNA search specificity. and create a GEM from them.  Then you can normalize the GEM and analyze with the at least one downstream workflows we have used in this course.  Be creative.

fasterq-dump SRR5139394
fasterq-dump SRR5139396
fasterq-dump SRR5139398

#Create and normalize the GEM

conda activate gemprep

git clone https://github.com/SystemsGenetics/GEMprep.git #If you don’t have cloned.

python ./GEMprep/bin/merge.py  SRR5139394.fastq SRR5139396.fastq SRR5139398.fastq merged--gem.txt

python ./GEMprep/bin/normalize.py merged--gem.txt merged--gem.log2.txt --log2

python ./GEMprep/bin/normalize.py merged--gem.log2.txt merged--gem.log2-quantile.txt --quantile

#Visualize the histogram

python ./GEMprep/bin/visualize.py merged--gem.log2.txt --density HIST-BEFORE.png

python ./GEMprep/bin/visualize.py merged--gem.log2-quantile.txt --density HIST-AFTER.png




