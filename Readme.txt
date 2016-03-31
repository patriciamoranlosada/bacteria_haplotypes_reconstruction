March 30th, 2016               README.txt

#**Bacterial haplotype reconstruction**#

AUTHORS: Patricia Morán Losada (MoranLosada.Patricia@mh-hannover.de)

##**Description**##

This approach identifies bacterial haplotypes defined by the length of identical sequence in pairwise comparisons of bacterial genomes. SNP synteny, i.e. the series of identical nucleotides at consecutive SNP positions, is taken as a measure of the haplotype. The metric is invariant regarding the reference genome chosen. Two matrices are constructed that contained columns of all quality-controlled SNPs ordered by genome position of the reference and rows of the bacterial isolates of interest. Only SNPs are considered that are shared by at least two strains of the panel. The outcome is the distribution of haplotypes in the analyzed strain sample that can be exploited to visualize the relatedness of clades in a tree. 

##**Usage**##

To run the four scripts of the pipeline, Perl and R must be installed.

The folder Bacterial_haplotype_reconstruction.tar.gz contains two subfolders. The subfolder 'Scripts' contains four perl scripts and two R scripts to run the pipeline. The subfolder 'Examples' contains the 5 files of the strains (input) in the 'Data' folder and output files in the 'Results' folder. The example shows haplotype analysis of five Staphylococcus aureus strains.

Script 1. 1_haplotypes_identification.pl : this script identifies all different haplotypes 
 
		INPUT: .txt file with all SNPs 
		INPUT FORMAT: Strain name\tSNP position\tNucleotide reference\tNucleotide strain 
				(the last 2 columns are optional)

		USAGE : perl 1_haplotypes_identification.pl
		
		OUTPUT: the script generates the following files:
			a)Haplotype_distances_hp.txt ->this file contains all different haplotypes for each pariwise comparison
					1st column: id value of the haplotype
					2nd column: value of syntenic SNPs
					3rd column: haplotype start position (based on the SNP number in the whole-genome
							SNP synteny)
					4th column: haplotype end position (based on the SNP number in the whole-genome 
							SNP synteny)
					5th column: names of the two compared strains
					6th column: haplotype start position (based on the nucleotide position in the 
							reference genome)
					7th column: haplotype end position (based on the nucleotide position in 
							the reference genome)
					8th column: names of the two compared strains
			
			b)TOTAL_MATRIX.txt ->it contains the matrix with all haplotypes of all [0.5 n x (n-1)] pairwise
						comparisons of n strains 
					1st row: SNP position in the reference genome
					2nd row: number of SNP
					3rd row and all following rows until the [0.5 n x (n-1)]th row: number of syntenic
						SNPs for that SNP in one of the in total [0.5 n x (n-1)] comparisons

Script 2. 2_matrix_rank_numbers_tree.pl : this script calculates the sum and median value of the haplotype rank numbers and generate two matrices.
			
		INPUT: Haplotype_distances_hp.txt, strains.txt and unique_consecutive_snps_sorted.txt 
			(generated in 1_haplotypes_identification.pl)
		
		USAGE: perl 2_matrix_rank_numbers_tree.pl

		OUTPUT: a new folder called 2_output_files is created which contains:
			a) Genome_distances_hp_rank_numbers.txt ->it contains the information of all haplotypes plus 
				an aditional column which contains the rank number value based on the syntenic SNPs value.
			b) matrix_median_ranks.txt -> it contains a matrix with the median rank number value for each 
				pair comparison
			c) matrix_sum_ranks.txt -> it contains a matrix with the sum of rank numbers for each pair comparison

Script 3. 3_similarity.pl: this script calculates the percentage of syntenic SNPs shared by two strains.
		
		INPUT: Haplotype_distances_hp.txt and TOTAL_MATRIX.txt (generated in 1_haplotypes_identification.pl)
		
		USAGE : perl 3_similarity

		OUTPUT: a new folder called 3_output_files is created which contains:
			a) sum_snps_shared.txt -> it contains the sum of syntenic SNPs shared by a pair comparison
			b) matrix_similarities.txt -> it contains the percentages of syntenic SNPs shared by two strains
			c) Similarity_strains.pdf -> plot to visualize the different percentages values 

Script 4. 4_Hb_distribution_plot.pl : the script plots the distribution of haplotypes along the reference genome.
	
		INPUT: Haplotype_distances_hp.txt (generated in 1_haplotypes_identification.pl)

		USAGE: perl 4_Hb_distribution_plot.pl

		OUTPUT: a new folder called 4_output_files is created which contains:
			a) Distribution_blocks_total.txt -> it contains the start and end reference genome position of 
				each haplotype
			b) Distribution_blocks_total.pdf -> a plot of the haplotypes distribution along the reference genome.
