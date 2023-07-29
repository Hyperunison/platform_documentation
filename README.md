# Unison Platform Documentation

## (1) Unison File Space and Workflows

## (2) Data Store

## (3) Data Analysis

## (4) Pipelines and workflows

## (5) Step-by-step-example: Run RNA-Seq analysis and Subsequent Differential Gene Expression Analysis


## Unison File Space and Workflows
Logon to the Unison platform:
Go to https://app.hyperunison.com/ an select the login option. Log on using
User: [your username]
Pass: [your password]
 
How to run a RNA-Seq primarly alignment in the GUI workflow:
[find old documentation and paste here]
 
Using Unison Jupyter Notebooks
 
Unison jupyter notebooks can be used for all types of data processing. A jupyter notebook can be created for any major programming language. It even is possible to use multiple programming languages within one jupyter notebook.
 
Unison jupyter notebooks are connected to the Unison file space. From the Unison file space, you can read files or save result files. Your code can be saved within the jupyter notebook.
 
Each jupyter notebook has a so-called kernel – a defined software environment. In the case of R-workbooks, we use the renv package manager to maintain required R-packages in a well defined and reproducible fully reproducible setting.
 
Unison projects are organized in a self-contained folder structure. The reason is to maintain each project in an easily transferable state. A defined folder structure makes it easier to draw in sub-workflows from the Unison code store or other sources.  
 
Structure of the Unison file-space
 
Each user has a personal file space. In that personal file-space you have a projects folder. Each project in the projects-folder is a self-contained unit of work. A project is organized around a larger research question and may contain several distinct data analysis projects, for example a genomic and proteomic analysis which then are integrated in a third analysis to create a multi-omics analysis.
 
Within a project folder you find the following folders:
/code folder: In this folder and it’s sub-folders all code and settings files are kept. This folder should not contain any data or results, so it remains small in file size and can be tracked and managed with a version-control service such as github. In the code folder you may have several subfolders containing the code for each sub-analyses, for example one folder with the code for the RNA-Seq analysis, one folder with the code for the proteomic analysis and one folder with the code for the multiomics integration analysis. Within the folder for an individual analysis, for example the RNA-seq analysis you find a design, documentation and settings folder to keep track of essential small files for this project. In addition you’ll find an analyses folder. The analysis folder contains modular units of work. In the case of the RNAseq analysis one part-analysis folder may constitute the nf_core_rnaseq_alignment (in a distinct folder if you perform this analysis on the command line), a downstreamt_analysis_1_DGE (differential gene expression) folder and additional folders for additional, related sub-analyses such as survival analyses, correlation analyses and so forth. In this sub-analysis folder you would also keep your analyses scripts, for example your jupyter notebooks.
 
/data folder: In the data folder you may keep large data files, particularly input files.
 
/results folder: In the results folder you would deposit result files of your analysis. We would recommend high-level html reports on the top level and create a report_figures and report_tables subfolder.
 
/workdir folder: We would suggest to use the workdir folder to do actual work. Here you can create temporary or intermediate files that don’t need to be tracked via a version control system.
 
 
 
 
 Specific for Alex case
 
·  	The bulk-RNA-seq analysis can be conducted via GUI, as described previously. You can select a relevant RNA-Seq dataset from the datastore, select the bulkRNAseq workflow and run the alignment with the nf-core RNA-seq pipeline. Make sure to specify the correct species for the alignment.
·  	Once the alignment analysis is complete, you can insert the file paths for the TPM and feature count matrices into the input section of the jupyter notebook for the downstream analysis.
·  	As examples, we have three jupyter notebooks for you to work with (1) one notebook containing your original analysis code (2) A notebook to perform a very basic differential gene expression analysis on two integrated melanoma RNA-seq datasets and (3) a notebook to access TCGA data for further analysis, for example to integrate it with your own data or to perform downstream analyses on the TCGA data only.
·  	The jupyter notebook with your (slightly refactored) code can be found at this path: /home/jovyan/projects/project05_Alex_melanoma_sing_scores/code/sing_score_workflow/analyses/sing_score_analysis/ sing_score_analysis_main.ipynb
·  	The jupyter notebook with an example DGE analysis of the Gide et al and Hugo et al. dataset can be found at this path: home/jovyan/projects/project02_rnaseq_federation/code/bulkRNAseq/analyses/downstream_analysis_1_DGE/Main_Analysis.ipynb
·  	The jupyter notebook with an example query for some melanoma TCGA RNAseq data can be found at this path: /home/jovyan/projects/project03_TCGA_data_import/code/TCGA_data_access/analyses/TCGA_gene_category_heatmaps/TCGA_heatmaps.ipynb
·  	A fourth jupyter notebook has been created to develop a distinct analysis workflow for you. It can be reviewed under this path: /home/jovyan/projects/project01_custom_workflow_Alex/code/Alex_custom_analysis/analyses/alex_custom_analysis_no1/alex_custom_analysis_V1.ipynb
It would be nice if you could add to this workbook ideas on what we can do together. It would be nice to create an end-to-end analysis workflow that’s meaningful to you.
·  	All of the above three notebooks work with the R422 kernel.
·  	To start a jupyter notebook, open the jupyter notebook within the Unison jupyter notebook server (to get there, click the data analyses tab within the Unison data analysis platform.
·  	To navigate to the correct folder, use the folder icon on the left-hand side of the screen.
·  	Once you have opened a jupyter notebook, the kernel should connect automatically. If it does not, select from the menu bar kernel > change kernel > select R422 from the pulldown menu.
·  	To run a single cell of code in the jupyter notebook, use the forward arrow in the menu bar. Alternatively, you can select from the menu Run > Run All cells
·  	If you have done some R-coding, the code should be familiar to you. You may edit any code cell and then re-run just that cell or or cells downstream of th

## (2) Data Store

## (3) Data Analysis

## (4) Pipelines and workflows

### Running the nf-core RNA-Seq pipeline

### RNA-Seq Differential gene expression workflow
Log on to the Unison platform (https://genobase.pro/) using the following credentials:
Username:        [your username]

Pass:             [your password]

Paste into the “Search by PRJ/GSE” search box in the top left corner the string “PRJEB23709”
Press enter and cklick on the dataset “Biomarkers of response and resistance to checkpoint blockade immunotherapy in metastatic melanoma”
Scroll down to the data processing results section
Please note: we ran the alignment RNA-seq pipeline on this dataset already. For this reason, you can review the results from this primary alignment run.
Let’s review the QC results before we move on to the differential gene expression analysis
Select the main files tab.
In the “Technical quality of the various sequencing files” section, click on the multiqc_report.html
This report contains a large number of plots assessing the quality of the RNA-seq samples (to be covered in detail elsewhere).
Once you’ve convinced yourself that this RNAseq experiment is of sufficient quality, let’s progress to the differential gene expression analysis.
Use the back button in the brower to get back to the previous menu.
Again, scroll down to the “Data processing results” section.
Click on the “Custom Differential Gene Expression” tab
You will need to specify two files in order to provide crucial details for the differential gene expression analysis: a design table specifying which samples are to be used for your differential gene expression comparisons and a model table that specifies the statistical model for that differential gene expression analysis.
Download the design.table.tsv file to your computer
Edit this file to specify the differential gene expression comparisons you need by adding a column named “comp_1”, “comp_2”, …, “comp_N” for each comparison you’d like to specify. In the GSE209801 example we already have a drug vs. ctrl comparison specified as “comp_1”.
In each column comp_1 to comp_N, indicate two sample groups by entering a group name for this group in this column. For a comparison Group_Perturbation vs. Group_Baseline, enter strings “1_Group_Perturbation” and “2_Group_Baseline” in the rows of the sample.ids you’d like to include in the comparison.
If you want, you can also delete samples that you deem irrelevant for your study (in this study, that might be the post-treatment samples, for example).
Save the design table after you have edited it.
Next, download and open the model.table.csv file. This table will specify the statistical model and the parameters to be used for the differential gene expression comparison.
Start by entering the comparison ID (“comp_1”, “comp_2”, …,“comp_N”) in the comparison ID column.
Type a name for each comparison into the comparison column. For example, this could be perturbationGroupName_vs_baselineGroupName. Please avoid empty spaces and use underscores.
For a differential gene expression comparison the test should be “Wald”. The type column should get the value “DGE”. The model column specifies the statistical model used for the comparison. If you don’t have any covariates or batch effects to consider, just enter “~ condition”. This will produce a differential gene expression comparison just comparing the perturbation and baseline group without considering any other effects.
For a DGE comparison, leave the reducedModel column empty
Enter either TRUE or FALSE in the normalizeAllSamplesTogether column. This will determine if only the samples required for the comparison are considered for normalization (=FALSE), or if all samples in the experiment are considered for normalization (=TRUE).
The betaPrior column determines if a logFC shrinkage algorithm is applied to the DGE results. It is either TRUE or FALSE. It can only be set to TRUE if the statistical model is just “~ condition”. If the model is more complex it has to be set to FALSE.
Save the model.table
Return to the custom Differential gene expression tab
Use the URL to design.table.tsv upload local file option. Specify the path to the design table on your computer that you have just edited. Upload the file.
Use the URL to model.table.tsv upload local file option. Specify the path to the model table on your computer that you have just edited. Upload the file.
Press button [RunGenerate report]
Use the link that appears (Pipeline started, view progress and result) to monitor progress. It should take only a few minutes for the results to appear.
Once the pipeline is completed, you can review the differential gene expression results in the “Result Files” tab
Click on the result files tab
Click on Differential gene expression > html_local
The main differential gene expression result report can be opened by clicking on “Main_RNAseq_Analysis.html”.
A differential gene expression spreadsheet can be found and downloaded in the report_tables folder. The file is called “DGEresulttable.txt”
Explanation Design File:
The design file defines, among other things, which samples are compared in a differential gene expression comparison.

The design file must have a column called “sample.id”. The strings in the sample.id column must match the column names in the count.tsv and the tpm.tsv files. The design file can contain fewer samples than are present in the count.tsv and tpm.tsv files.

Column name should not contain only letter and underscore [] and shoud not start from number. Regex: [a-zA-Z_]\\w+

The design file must contain a column labelled “sample.group”. In the sample group column replicates of the same type are summarized by the same string. For example, if the sample names are drugTreatment_rep1 and drugTreatment_rep2, the sample.group entry for both would be “drugTreatment”.

The differential gene expression comparisons are specified in additional columns. For each differential gene expression comparison one column must be present in the design file. For example, the first differential gene expression column could be called “comp_1”. In the comp_1 column two types of string should be found (they need to be exactly the same, and there should be no more than two different strings). The first string will specify the first group in the DGE comparison, the second string the second group. For example, the first string could be “1_DrugTreatment” and should be entered in all rows with a sample of the drug treatment group that’s to be used in the DGE comparison. The second string could be “2_Control”. This will define the second group of samples used in the DGE comparison. In the above example a comparison of DrugTreatment vs Control would be generated. It is important that the first group of the comparison is prefixed with “1_” and the second group with “2_”.

Additionally, the design file could contain column names prefixed with “LRT_”. This will specify and trigger an LRT multi group test. In an LRT column, multiple sample groups can be defined with a string. [To keep it simple, let’s forget about this feature for now.].

Explanation Model table:
The model table specifies the statistical models to be used for differential gene expression and LRT tests. There should be one row in this file for each DGE comparison and for each LRT column.

comparison column

The comparison column entry is constructed for each DGE comparison by concatenating the “1_ABC” and “2_CDE” group string to “ABC_vs_CDE”. It is the name with which the comparison appears in the report.

For LRT in may be any value, but for the same group string must be the same, and for different group it should be different. In the simplest way, it may be the same as group name

comparisonID

The comparisonID column defines the column in the design file from which a DGE comparison is constructed. It needs to be column name that is present in the desing file.

Regex: (comp_\\d+|LRT_\\w+)

test

The test column will contain [for now] only two entries: “Wald” for a DGE comparison and “LRT” for an LRT test.

This field may be hidden now, until we have to other values except “Wald” AND “LRT”

Regex: Wald|LRT

type

The type column will be “DGE” for differential gene expression or “LRT” to specify an LRT test.

Regex: DGE|LRT

model

The model column defines the statistical model used for the DGE or LRT comparison. It is a formula that always starts with “~” followed by factors of the statistical model. Each factor string must be a column name in the design file, with one exception: the condition entry is used to indicate the comparison itself, and is NOT a column in the design file. However, all other strings appearing in the design formula MUST be column names in the design. In the statistical model, those factors/column names can appear in various combinations. For example it could look like this “~ Age + DrugTreatment + Gender + condition” or “~Age + DrugTreatment:Gender” or “~Age + DrugTreatment*Gender”. In the simplest case, the default option “~ condition” should be offered. In this case the simplest possible statistical model will be used.

It may be any mathematical formula, using +, *, brackets and other symbols

reducedModel

The reducedModel column is only relevant for LRT tests. The same rules as for the model column apply.

It may be any mathematical formula, but it may not contain any field not present in model

normalizeAllSamplesTogether

The normalizeAllSamplesTogether column indicates whether all samples in the design file, or just the samples specified for the DGE/LRT comparison are normalized together before the DGE/LRT comparison. This entry is Boolean, TRUE or FALSE

betaPrior

The betaPrior column determines whether log-fold-shrinkage is applied after the DGE comparison. It is only relevant for type = DGE. If the entry in the model column is NOT just ~ condition, this entry must be set to FALSE. However, if the model column contains just “~ condition” or “~ age” or “~ anySingleFactor”, it should be TRUE.

If model is “~condition“ it may be both TRUE and FALSE. And only in this case

## (5) Step-by-step-example: Run RNA-Seq analysis and Subsequent Differential Gene Expression Analysis

To illustrate the various functionalies of the Unison data analysis platform, we are going to perform a differential gene expression analysis to determine which genes are perturbed by an experimental drug. 

### Find a public dataset of interest
If you have seen a dataset of interest in a publication, try to find the GEO accession number for that in the article. The GEO accession number is a number in the format GSE123456. If you have such GEO dataset identier, you can proceed the select dataset from the Unison data store point below. 

To find a public dataset of interest, head to the [GEO website](https://www.ncbi.nlm.nih.gov/geo/). In this example, we will be searching for "Transcriptome changes after NEN treament in neuroblastoma cell". If you click on the hit dataset, you can determine the GEO identifier we need [link to resultpage](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE209801): It is GSE209801. 

With this identifier in hand, we head for the Unison data store.

### Select Dataset from the Unison data store
Once you are loged onto the Unison platform, the data store can be accessed from the [main page](https://app.hyperunison.com/) by selecting the Data Store option. This will take us to the Unison data store. In the search box we enter the GEO gene set identifier we obtained before: GSE209801. Just one dataset should be diplayed and we click on this tile to lead us to the [Unison dataset detail page for this dataset](https://app.hyperunison.com/datasets/GSE209801). 

### Submit the RNA-Seq dataset to the alignment pipeline
If you are happy with the dataset selection and have confirmed that the dataset is suitable for being processed by an RNA-Sequencing analysis workflow, you can initate running the RNA-Sequencing analysis workflow. 

!!! Note to us: This section needs tidying up. Many items with unclear functionality !!!

To do so, select in the Data Analysis Options section the "Unison Cluster (login required)" option. Then click on the green button "Run nfcore/rnaseq" on the right hand side. You will be taken to a menu in which the nf-core rnaseq pipeline, which we are going to use for the alignment step, will be configured. 

It is important to specify the correct reference transcriptome and genome. In our example, we are dealing with a human dataset. The simplest way to get the analysis started is to set the
`--genome` option to "GRCh38". 

!!! Note to us: Add section on how to use the `--fasta` and `--gtf` flags to specify reference transcriptome / genomes. In this case we need to explain in more detail (a) how the user finds and selects those references and (b) how the downstream analyses need to be adapted as the output matrix of the nf-core pipeline will contain gene identifiers, rather than gene names. This will then have to be matched to the same version of reference transcriptome in order to convert them into gene names. !!!
!!! More documentation needed on how to deal with other species than human !!!

Next, set the ```--aligner``` flag to ```star_rsem``` from the pulldown menu. 

!!! Note: At present users should not select the salmon_rsem aligner option, as it consistently fails !!!

For a basic analysis, the other parameters can be left in their default state. 

Now press the green Run pipeline button at the bottom of the page. 

The next step is to edit the sample sheet and asign biologically meaningful sample names. This will make it a lot easier to work with the downstream analysis. 

By default the Unison platform provides you with identifiers for all samples. The first thing you need to check is the Unison guess on the strandedness of the experiment is correct. You can best do this by reviewing which sequencing kit has been used in the library preparation for this RNA-Seq dataset. It can be done on the [GEO detail page for this dataset](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE209801). As sequencing platform we find an Illumina machine - so we can guess that sequencing was done in the reverse mode. To definitely confirm this guess we would have to go to the original research article and determine which sequencing kit was used. With that information we can determine if the kit is operating an unstranded, forward or reverse protocol. 

Now we can determine biologically meaningful sample names. To do that, we will have to retrieve the meta-data information for this experiment. We do this by visiting the [GEO detail page for this dataset](), scrolling to the bottom and downloading the "Series Matrix File(s)" from the "Download Family" tab. Once you have downloaded the file, open it in a program like Excel. Scroll down in the first column until you find the entry "!Sample_relation". There are often several rows with this entry. Find the row in which the sample identifiers are matching the ones given in the Unison sample sheet. In this case it will be the row with entries in this format "BioSample: https://www.ncbi.nlm.nih.gov/biosample/SAMN29981293". The column with this sample identifier will contain more information about this sample. For example, we see that for this sample an entry "treatment: DMSO" is present. This means this sample was treated with DMSO. We can do this for each sample and determine the following biological sample names based on the treatment regime:

Original entries:

|UnisonId |sample alias|sampleId|strandedness| 
|-----|------------|------------|-------|
|97851|SAMN29981289|SAMN29981289|reverse |
|97852|SAMN29981290|SAMN29981290|reverse |
|97850|SAMN29981288|SAMN29981288|reverse |
|97853|SAMN29981291|SAMN29981291|reverse |
|97854|SAMN29981292|SAMN29981292|reverse |
|97855|SAMN29981293|SAMN29981293|reverse |

Change to these biologically meaningful sample names based on the series.matrix file entries for this experiment

Edited entries:

| UnisonId | sample alias | sampleId | strandedness | 
|-----|------------|------------|-------|
|97851	| SAMN29981289	| NEN_treatment_Rep1 |	reverse |
|97852	| SAMN29981290	| NEN_treatment_Rep2 |	reverse |
|97850	| SAMN29981288	| NEN_treatment_Rep3 |	reverse |
|97853	| SAMN29981291	| DMSO_Rep1 |	reverse |
|97854	| SAMN29981292	| DMSO_Rep2 |	reverse |
|97855 |	SAMN29981293	| DMSO_Rep3 |	reverse |

Once you've made the above edits, you need to either select or create a folder for the outputs of this analysis pipeline. You can do this by selecting an existing folder from the dropdown menu or by creating a new folder. For this demonstration, we select the 'GSE209801_rsem_start_GRCh38_test' option. After that we press the green Start pipelne button. 

Progress of the pipeline run can be monitored on the page that opens. It will take a couple of hours for the RNA-Seq pipeline run to complete. 

!!! Note: Add more information here on trouble shooting and explanations of the various tabs. !!!

### Check Quality of your sequencing samples

Once your running process is completed, go to the result files tab. In the result files tab you should see a "NfCorePipelineRnaseq" folder. Click on that folder and open the multiqc_report.html file. This document contains a number of relevant quality control measurements. Convince yourself that the sequencing quality was good. 

### Perform downstream processing to create the downstream differential gene expression analysis and visualization of results

#### Find file paths for input files for downstream analysis
In order to perform the downstream analysis, we will have to find the file paths to files we need for the downstream processing. 

The first file we need to locate is the RSEM feature count matrix. To do that, we open the terminal via the Data Analysis tab. Click the data analysis tab, then from the "Other menu" click on the terminal tile. 

Now type 
```cd ~```
To make sure you are in your home directory. To find the output data, switch to the data folder by typing 
``` cd data```
Then type
```ls``` 
to see all folders in that directory. 

You should see a folder that has the same name as you had choosen earlier in the analysis, in the case of this example "GSE209801_rsem_start_GRCh38_test". Within that folder you find a data filder. Within that folder you find a folder specifying the piplinename, in this case RnaSeqpublicpipeline. Within that folder you find the name of the dataset, in this case it's shown by it's biosample ID, rather than it's GEO ID, so here dataset GSE209801 is represented as PRJNA8652500. Next we select the run id and finally the folder with the output files, NfCorePipelineRnaSeq. 

The full path to the result files for this analysis is in this case:
```/home/jovyan/data/GSE209801_rsem_start_GRCh38_test/data/RnaSeqpublicpipeline/PRJNA862500/run458/NfCorePipelineRnaseq```

We move into this folder
```cd /home/jovyan/data/GSE209801_rsem_start_GRCh38_test/data/RnaSeqpublicpipeline/PRJNA862500/run458/NfCorePipelineRnaseq```
and review available files
```ls```

We see the following files:
| File name |
--------------------------------
| rsem.merged.gene_counts.tsv |
| rsem.merged.gene_tpm.tsv |
| samplesheet.csv |
| multiqc_report.html |
| rsem.merged.transcript_tpm.tsv |

As input files we will need the files rsem.merged.gene_tpm.tsv and rsem.merged.gene_counts.tsv in this case.

So we note as full filepaths:
```/home/jovyan/data/GSE209801_rsem_start_GRCh38_test/data/RnaSeqpublicpipeline/PRJNA862500/run458/NfCorePipelineRnaseq/rsem.merged.gene_counts.tsv ```
and
```/home/jovyan/data/GSE209801_rsem_start_GRCh38_test/data/RnaSeqpublicpipeline/PRJNA862500/run458/NfCorePipelineRnaseq/sem.merged.gene_tpm.tsv```

As basis for the design file that we need to create, we will use the sample sheet. As filepath for that we note down:
```/home/jovyan/data/GSE209801_rsem_start_GRCh38_test/data/RnaSeqpublicpipeline/PRJNA862500/run458/NfCorePipelineRnaseq/samplesheet.csv```
