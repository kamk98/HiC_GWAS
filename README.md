# HiC_GWAS


## Aim
To study the statistical significance of the association between cancerous, non-cancerous traits and SNPs (Single-nucleotide Polymorphism) in relation to TAD boundaries. 


## Background
This was a project I had worked on as a Graduate Research Assistant under Dr. Benjamin Soibam at University of Houston - Downtown, over the summer of 2023. This was a comparitive study of a paper that was already published. The professor aimed to include more features in the genome-wide association study, and the work done so far was a starting point. Since my tenure as a Research Assistant ended at the end of August, I could not continue assisting him further in the study, hence, I had submitted a comprehensive report of the work I had done so far, along with the code (jupyter).

## About the datasets
1)	**GWAS Catalog**

**Note**: Could not upload in the repository due to the 25 MB file size limit. Provided instructions to download them accordingly. 

A GWAS catalog (Genome-Wide Association Study catalog) is a curated collection of results from genome-wide association studies. GWAS is a type of study that scans the genomes of a large number of individuals to identify genetic variations associated with specific traits, diseases, or conditions. For this study, we utilize the gwas catalog provided by University of California Santa Cruz’s genome browser.

Link: https://genome.ucsc.edu/cgibin/hgTables?db=hg19&hgta_group=phenDis&hgta_track=gwasCatalog&hgta_table=gwasCatalog&hgta_doSchema=describe+table+schema

Click the above link and follow the steps below:
(i)	Hover over the ‘Download’ option on the upper tab and select “Genome Data”.
(ii)	Click on “Human”.
(iii)	Scroll down and you will come across the hg19 section. Under this section, click on ‘Data Archive’ present at the bottom of the list.
(iv)	Select ‘GWAS_Catalog/’.
(v)	Below ‘Parent Directory’, you will see 13 folders. For each of the 13 folders, click on the file that ends with ‘txt.gz’ extension.
(vi)	After downloading all of them onto your system, extract them all and you will see .txt files for each one of them.
In your jupyter notebook environment, create a directory (optional) called ‘Research Assistantship’. This is where you will be storing your main code file along with the HiC and GWAS datasets.
Within Research Assistantship, create a directory called ‘GWAS_Data’ and upload the obtained 13 gwas catalogs here.


2)	**HiC datasets**

We use the HiC datasets provided by University of Miami’s Computer Science department (at 10kb resolution). We made use of 5 cell lines, namely: GM12878, IMR90, HMEC, NHEK and HUVEC. 

Link:
http://dna.cs.miami.edu/TADKB/

Click the above link and follow the steps below:
(i)	Click on ‘Download’ from the upper tab. 
(ii)	Select ‘TAD_annotations.tar.gz’.
(iii)	Within the downloaded folder, you will see 90 files. Select the files for the aforementioned 5 cell lines. Make sure to select the 10kb files which have the ‘DI’, ‘GMAP’ and ‘IS’ abbreviations. These abbreviations refer to different methods or experimental techniques used for studying chromosomal interactions and the three-dimensional organization of the genome. There will be 15 files on the whole and for your convenience, save them in a separate folder.
In your jupyter notebook, within ‘Research Assistantship’, create a directory called ‘HiC Data’ and upload the 15 HiC datasets here.

## Data Analysis Workflow
### Data Preprocessing:
(i)	First, create a new directory within the ‘Research Assistantship’ directory called ‘New_HiC_Data’, the reason for which will be explained in the following passage:

We define TAD borders as regions of 20  kb located inside the TAD. For this, we implement the necessary changes in all the 15 HiC datasets, i.e., defining TAD borders, and save these newly obtained datasets into the ‘New_HiC_Data’ directory created recently. I have added ‘MK_’ before the titles of the original HiC datasets for the new datasets. The code for this is present in the jupyter notebook.
(ii)	In this step we will be dealing with the 13 gwas catalogs. We will run a block of code to create a single new dataframe called ‘MK_GWAS_Catalog’, which comprises of the union of all the 13 gwas catalogs, thereby dropping any duplicate rows. This new ‘MK_GWAS_Catalog’ dataset will be saved in the ‘GWAS_Data’ directory.

(iii)	The HiC datasets have 23 unique chromosome types, as is expected. However, in the gwas catalog, there seem to be extensions of the original 23 chromosome types and a ‘chrY’. We first remove the rows comprising of ‘chrY’. Following this, we implement a regular expression operation to modify the extensions to their original chromosome types.  

(iv)	In order to differentiate between cancerous and non-cancerous traits in the gwas catalog, we make use of a regular expression to check if the values in the ‘trait’ column contain the word ‘cancer’ (case insensitive). This checking helps create a new column ‘contains_cancer’ which would comprise of Boolean values i.e., True or False.

### Hypergeometric Testing:
Testing a preferential location in TAD borders of the SNPs associated to a given disease involves the hypergeometric distribution H (q | N, n, Q) describing the 
probability of getting q border SNPs when drawing Q elements (as many as the number of SNPs associated to the considered disease) at random, without replacement, in the null model. In the SNP-based null model, close to that considered, e.g., in [57], N is the total number of disease-associated SNPs listed in the GWAS catalog and n the part located in TAD borders. 

In other words:
N: Total number of entries in the gwas catalog..

Q: Number of SNPs associated with a particular trait.

n: Number of base pairs in TAD borders (sum the differences between 'chromStart' and 'chromEnd' in the HiC dataset).

q: Number of SNPs within TAD borders for a particular trait.

We perform the hypergeometric testing for cancerous and non-cancerous traits separately.


Note: A p-value less than 0.05 in a SNP-based test for TAD (Topologically Associating Domain) border enrichment suggests that there is a statistically significant association between the SNP and the trait in relation to TAD boundaries. In other words, the test results indicate that the SNP's presence or variation is linked to the observed trait's association with TAD boundaries.

### Findings:
**Cancerous Traits**:
Proportions range from approximately 96.61% to 99.4%.
The highest proportion is 99.4% in several HiC files.

**Non-Cancerous Traits**:
Proportions range from approximately 96.61% to 98.66%.
The highest proportion is 98.66%.

**Overlap in Proportions**:
There is an overlap in the proportions of cancerous and non-cancerous traits with SNP-based test p-values less than 0.05.
Both cancerous and non-cancerous traits exhibit high proportions, making it challenging to distinguish them solely based on this criterion.

**Impact of Class Imbalance**:
Considering that only 5% of the traits are cancerous, the high proportions suggest that cancerous traits are well-represented among the significant traits. However, the overlap indicates that non-cancerous traits also exhibit significant SNP-based test p-values.

I believe that a HiC dataset with a more balanced distribution might be beneficial to come towards a more concrete conclusion. I will try to search for other such datasets from different sources, or explore techniques for handling class imbalance, such as oversampling the minority class or using algorithms designed for imbalanced datasets.
