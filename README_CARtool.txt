##########################
CARtool v1.0
Anna Odelgard, March 2019

##########################
Description
CAR tool is a tool for assessment of per base quality of NGS data. 
The tool generates lists, tables and figures that can be used to evaluate coverage depth and breadth over regions of interest. 
CAR tool is especially created as a coverage tools for clinical use, it is a comprehensive and easy to use tool for both clinicians and bioinformatics. 
Multiple optional settings adapt the output based on the users’ needs with lists, tables and figures.

Example run: Coverage Analysis Report tool is launched from a terminal window.
$ python ProgramLancher.py -a myRegions.bed -b myReads.bam -c 30 50 70 -e NameOfUser -o OUTfolder OUTfilename



##########################
Dependencies:
Samtools – for per base coverage depth calculations.

##########################
Mandatory input:

-b 1 BAM file with the reads
-c 3 coverage depth threshold values (for example 10 20 100)
-e Name of the person running the analysis, added to the log file
-o Output folder and name of the output files
-a 1 BED file containing the regions of interest (ROI). 
The .bed file of the regions should contain chromosome, start position, end position and region name in that order. 
The Start position is 0-based and the end position 1-based.
Additional functionality can be triggered if the (a).bed file is formatted appropriately 
If the first part of the bed region RegionName are the same, followed by a dot (.), regions may be merged in the report. 
Similarly exon and transcript information can be parsed if the following format is used:
RegionName.exon.#.transcript.#.chr#...... Other words
the order of the exon number and transcript must be as the order above.

##########################
Optional input:
-p Phred score and mapQ filtering of the bam file. Scores below the chosen values will not be used
for the coverage calculation. With option (-p all value1 value2) the filtered BAM file is used
throughout the analysis while (-p value1 value2) is used only for an additional column in the statistics table.

-k Combine rows in the BED file, the rows in the bed file that has the same for example gene name
will be combined and the tables and figures will be calculated on per gene information. For this
option the region name must be placed first followed by a dot.

-f Create figures (images are only generated if this flag is used).

-s Strand specificity, this flag will generate two additional statistics tables; one with only reads from the positive strand 
and one with only reads from the negative strand. All other computations are done regardless of the strand type.

-v Validation, creates an additional column in the statistics table. A star indicates that the gene or ROI
had a coverage breadth below 95% at the first threshold value for the statistics table. This will also
generate a validation list with all those genes or ROIs below 95%.

-t Enter a hot spot list of base pairs of interest. These positions will be pointed out in the region
figure by arrows. The hot spot list should be a BED file with the columns chromosome, start, stop and
region name.

-m Adds exon and transcript information to the mean lists. Make sure that the region name is written as follows; 
RegionName.exon.#.transcript.#.chr#...... 
Other words than exon and transcript is okay to use but the order of the exon number and transcript must be as the order above.

-l Enter a list of regions that are known to be low. A column in the mean short list and mean full list is
added that indicate if the regions are usually low. And the table in the Region plot will have the first
column in the row colored red. The input list should be a BED file with the columns chromosome,
start, stop and region name.

-d Returns the detailed per base pair coverage list

-i Tailor the command sent to Samtools to calculate coverage depth. The command will end with
adding the bam and bed files and saving the resulting file. Observe that Samtools uses a cut off value
for the reads that effects the coverage if more reads are in the bam file. CAR tools set the cut of
value to 30000.
For example the default is set to be “samtools depth -a -d 30000”. This string can be changed by the
user in the input followed by the flag -i. The new string is then merged with -b theRegionsfile.bed
Reads.bam, and the output is saved to the correct file and folder.

##########################
Standard output:

Full mean list of sections below and over the first entered coverage threshold value. All base pair
coverage values are evaluated for every ROI or gene and are used to create new sub regions. The sub
region consists of all adjacent base pair position either above or under the threshold. For each sub
region the coverage mean is calculated.

Short mean list is extracted from the full mean list. Only sub regions below the coverage threshold
are saved in this shorter list. If the ROI or gene has no sub regions below the coverage threshold they
will not be in the list.

Statistics table with coverage breadth values for each ROI or gene. The coverage breadth is
calculated from the 3 threshold values in the input. The original statistics table is not strand specific,
but if this choice is activated two additional statistical tables one for each option are generated.

Log file, a record of what files that was used, who run the analysis, used settings together with
calculations of the total coverage mean value and total mean of coverage breadth values.

##########################
Optional output:
Validation list, list of ROIs that has less than 95% coverage in the first column of the statistics table.
These are marked with stars in the statistics table if the validation option is turned on.

Strand specific tables, two additional statistics table is generated one for only positive reads and one
for only negative reads. The last column in each table indicates the difference in coverage breadth.

Phred score and mapQ filtering of the bam file. The filtered coverage depths are either used for the
whole analysis or only in the statistics table, added as a row under each ROI. The option, -p all value1
value2, is used if the whole analysis is to be run with the filtered bam file while -p value1 value2 only
uses the filtered bam file as additional rows in the statistics table.

Figures
A pie chart of positions above or under the coverage threshold per ROI.
A region plot that visualize where the coverage is lower than the threshold together with a table of
the low regions name, chromosome, start position, stop position, mean and length.
A bar plot with low coverage exons displayed as bars with amount of positions above or under the
coverage threshold. These figured are only created if combine rows is activated.

