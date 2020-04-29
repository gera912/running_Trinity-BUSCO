# Evaluating de novo Transcriptome assemblies (Part 2)
This week we will learn how to run Trinity (without actually running it), and evaluate different Trinity assemblies based on read sets filtered and/or normalized differently.
Please work with the following Trinity output found on Talapas here:
```
/projects/bgmp/shared/Bi623/assign5/tri_rare.fasta
/projects/bgmp/shared/Bi623/assign5/tri_rare.timing 
/projects/bgmp/shared/Bi623/assign5/tri_40Xnorm.fasta 
/projects/bgmp/shared/Bi623/assign5/tri_40Xnorm.timing

```
1.	The files with “rare” in the name are from a Trinity assembly based on the “cleaned, rare-kmer-filtered only” Gulf pipefish reads from Assignment 5. The files with “40Xnorm” are output from a Trinity run based on the cleaned, rare-kmer-filtered, 40X-normalized reads. Look at the information in the .timing files. Do any runtime differences between the rare and 40Xnorm assemblies stand out?
2.	We don’t have time to run Trinity (hence the files above), but it is actually very easy to run, with limited flexibility in user choice of parameters. Below are the options I used for the above assemblies. Consult the Trinity documentation https://github.com/trinityrnaseq/trinityrnaseq/wiki, and briefly explain the options below.
```
Trinity.pl --seqType fq --JM 50G --left $work/SscoPEcleanfil_R1.fq \
--right $work/SscoPEcleanfil_R2.fq --output $work/assembly --CPU 10 \
--min_contig_length 300 --min_kmer_cov 3 --group_pairs_distance  800
```

3.	Using information in the two assembly files, calculate the number of transcripts, the maximum transcript length, the minimum transcript length, the mean transcript length, and the median transcript length. To do this you can produce a file for each assembly that contains transcript lengths, then import into R for analysis. Regardless of how you tally the statistics, plot the contig length distributions for each assembly using R hist() and boxplot() functions. Make sure your plots are clean and labeled appropriately.
4.	Based on your assembly statistics and what you know about transcripts, comment on whether there is a clear difference in quality between the two Trinity assemblies. Also comment on differences in the total number of transcripts and transcript groups (“loci”) for the two assemblies. For this last question, you’ll have to do a bit of work on the transcript header lines with UNIX commands. HWhat might transcripts within a “locus” represent? (hint: “compX_cX” prefixes represent distinct transcript bundles (“loci”)).
5.	BLAST your pipefish transcripts from each assembly (as separate jobs) against threespine stickleback protein sequences from Ensembl. You should already have a stickleback blast database from Assignment 3.

- a.	Since your pipefish data are nucleotides, and the stickleback data are amino acids, you will need to use a translating BLAST program. Research the options to figure out the appropriate program. This will take longer to run than the standard BLASTp or BLASTn. Only consider hits with an e-value less than 1x10-5, and retain only the best stickleback hit for each transcript.
6.	For each top stickleback BLAST hit you identify, obtain the gene name (e.g. ENSGACG00000000002, etc.) from the sequence headers in the Ensembl stickleback proteins .fasta file. Produce a table of the top BLAST hits you identified for each pipefish transcript, including the trinity transcript ID, stickleback blast hit e-value, stickleback blast hit protein ID, and stickleback hit gene ID. Are there more unique stickleback gene hits for one assembly vs. the other?
7.	Optional: Try to run BUSCO on both tri_rare.fasta and tri_40Xnorm.fasta transcriptome assemblies and include the reports (see lecture). Does the BUSCO analysis imply one assembly is more complete than the other? Does this evidence align with the above assessments?

