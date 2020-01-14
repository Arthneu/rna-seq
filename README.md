# rna-seq
my first time to create an rna-seq protocal
software preparation (conda install: hisat2, samtools install with R: ballgown
step1: download SRA date
$ wget -c http://
step2: download teh genome file and annotation file 
$ wget -c https://
step3: download genome index  E.g. human
$ wget -c ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/data/hg19.tar.gz
if genome index cann't be find, you can use hisat2 build an index file
step4: Mapping to genome (here use hisat2)
$ hisat2 -p 10 -x /date/homo_sapiens/rna_seq/reference/index/hg19/genome_index -1 /date/homo_sapiens/rna_seq/SRR1542484_1.fastq -2 /date/homo_sapiens/rna_seq/SRR1542484_2fastq -S /date/homo_sapiens/rna_seq/SRR1542484.sam 2>> mapping_repo.txt
step5: use samtools to convert sam file to bam file (inorder to save space)
$ samtools sort -@ 8 -o SRR1542484.bam SRR1542484.sam
step6: stringtie
$ stringtie -p 10 -G /ref/Mus_musculus/UCSC/mm10/Annotation/Genes/genes.gtf -o [output_gtf_file] -l [filename] [input_bam_file]
step7: merge (this step is needed when processing mutiple sets of date)
stringtie --merge -p 10 -G /ref/Mus_musculus/UCSC/mm10/Annotation/Genes/genes.gtf  -o stringtie_merged.gtf mergelist.txt
step8: ballgown
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/[file_name]/output_merge.gtf [input_bam_file]
