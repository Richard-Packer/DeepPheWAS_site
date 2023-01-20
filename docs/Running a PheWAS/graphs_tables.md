---
layout: default
title: Graphs and tables
nav_order: 5
parent: PheWAS scripts and examples
---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>


Now all the results should be available it is time to convert them into tables and graphs. The code in the examples can be used to produce graphs and tables the all the example results produced with minor edits.


## 05_tables_graphs.R

Creates result tables and graphs for the association analyses from either the **R** or **PLINK** methods. The two usages below represent inputting data from `03a_PLINK_association_testing.R` and `03b_R_association_testing.R`.
To produce graphs input the `--per_group_name_graph`, `--per_snp_graph` or `--R_association_graph` arguments depending on result source and transformation of the data required. There are many options to select related to filtering for MAC in PLINK results and general appearance of the graphs. By using the provided filter inputs it is possible to edit any individual graphs using the array of options.

**Usage**
**PLINK results**
```bash
05_tables_graphs.R (--results_file=<FILE> --analysis_name=<name> --plink_results --SNP_list=<FILE> 
--save_folder=<FOLDER>) [ --group_filter=<text> --PheWAS_ID_filter=<FILE> --PheWAS_manifest_overide=<FILE> 
--max_pheno=<number> --sig_FDR=<number> --no_save_table_all_results --no_graph_all --no_graph_sig 
--max_FDR_graph=<number> --SNP_filter=<FILE> --group_name_filter=<FILE> --save_raw_plink --MAC=<number> 
--MAC_case=<number> --MAC_control=<number> --per_group_name_graph --per_snp_graph --save_table_per_group_name 
--save_table_per_snp --sex_split --PheWAS_ID_label_filter=<FILE> --max_overlap_labels=<number> 
--graph_file_save=<name> --label_text_size=<number> --order_groups_alphabetically --order_phenotypes_alphabetically 
--save_all_graphs]
```

**R association results**
``` bash
05_tables_graphs.R (--results_file=<FILE> --analysis_name=<name> --R_association_results --save_folder=<FOLDER>)
[--group_filter=<text> --PheWAS_ID_filter=<FILE> --PheWAS_manifest_overide=<FILE> --max_pheno=<number> 
--sig_FDR=<number> --no_save_table_all_results --no_graph_all --no_graph_sig --max_FDR_graph=<number> 
--R_association_graph --sex_split --PheWAS_ID_label_filter=<FILE> --max_overlap_labels=<number> 
--graph_file_save=<name> --label_text_size=<number> --order_groups_alphabetically --order_phenotypes_alphabetically 
--save_all_graphs]
```

For full details of all of the arguments please use the help argument `-h`. 
Below we describe the column names for each for tables produced from `03a_PLINK_association_testing.R` and `03b_R_association_testing.R` and the describe the features of the two graphs produced **all_pheno** and **sig_pheno**.

---

### Table key - PLINK results
Results tables from `03a_PLINK_association_testing.R` data, have the following columns.
* **group**: Grouping variable used to subset the analysis, classically this will be ancestry.
* **collective_name**: Name taken from the SNP_list group_name input. Used to identify analysis and also group together multiple SNPs for per-grouping_name analysis. 
* **PheWAS_ID**: Phenotype ID.
* **category**: type of phenotype, indicates how it was made and what sort of data it used.
* **description**: description of the phenotype.
* **N_ID**: Total number of participants in the analysis.
* **rsid**: SNP identifier.
* **FDR**: False discovery rate.
* **P**: P value.
* **OR**: Odds ratio.
* **Beta**: Raw Beta value from the analysis.
* **L95**: lower 95% confidence interval.
* **U95**: upper 95% confidence interval.
* **coded_allele**: the allele which the direction of effect is based (the tested allele).
* **non_coded_allele**: the other allele.
* **minor_allele**: the allele with the lowest count/frequency of the two.
* **MAF**: Minor allele frequency.
* **MAC**: Minor allele count.
* **MAC_cases**: the minor allele count in cases only.
* **MAC_controls**: the minor allele count in controls only.
* **chromosome**: The chromosome of the SNP.
* **position**: The genomic position of the SNP.
* **Z_T_STAT**: The Z or T stat (depending on binary or quantitative phenotype).
* **SE**: Standard error. 
* **effect_direction**: The direction of effect (positive or negative)
* **phenotype_group**: Major grouping of phenotype used for both graphs.
* **phenotype_group_narrow**: More specific grouping only used in tables.
* **short_desc**: The short description used in the graphs rather than full description seen in the table.
* **Info_score**: INFO score shows the confidence of the call of the SNP if it was imputed.
* **firth**: Whether Firth regression was used by PLINK.
* **TEST**: The genetic model used.
* **Error_flag**: the recorded error flag from PLINK.
* **ID**: the created ID of the SNP used by appending the rsid and with the two alleles in alphabetical order.
* **graph_save_name**: the save name for the graph produced from the SNP taken from SNP_list.
* **sex_pheno_identifier**: identifies whether the phenotype is sex stratified. 

---

### Table key - R association results
Results tables from `03b_R_association_testing.R` data, have the following columns.
* **name**: Name of the analysis, for GRS/PRS refers to the trait for other inputs refers to the name given to the specific column the data is stored under used when running 03b_R_association_testing.R.
* **PheWAS_ID**: Phenotype ID.
* **category**: type of phenotype, indicates how it was made and what sort of data it used.
* **phenotype**: description of the phenotype.
* **FDR**: False discovery rate.
* **P**: P value.
* **OR**: Odds ratio.
* **Beta**: Raw Beta value from the analysis.
* **L95**: lower 95% confidence interval.
* **U95**: upeer 95% confidence interval.
* **SE**: Standard error. 
* **phenotype_group**: Major grouping of phenotype used for both graphs.
* **group_narrow**: More specific grouping only used in tables.
* **short_desc**: The short description used in the graphs rather than full description seen in the table.
* **effect_direction**: The direction of effect (positive or negative)
* **group**: Grouping variable used to subset the analysis, classically this will be ancestry.
* **name_group**: concatenated name and group, used for file saving.
* **sex_pheno_identifier**: identifies whether the phenotype is sex stratified. 

---

### Graphs key
#### All_pheno graph
Graphs with suffix **all_pheno** are graphs showing all the association results. The Y axis is -log10(FDR) with a line of significance in red change this by using argument `--sig_FDR`. The x-axis has the phenotype_group and by default orders graph by the lowest association within each group and then in descending order per group, this can be changed using optional arguments `--order_groups_alphabetically` or `--order_phenotypes_alphabetically`. The direction of effect is signified by a triangle either pointing up or down. Phenotypes that have associations deemed significant are labelled using the **short_desc** within the tables. The value in parenthesis is the category of phenotype seen in the tables, this also indicates whether a phenotype is male stratified (male), female stratified (female) or age of onset (age_of_onset). In some circumstances the number of significant associations exceeds what is practical to label and the figure can become cluttered, alter the `--max_overlap_labels` argument to alter the density of labels or `--PheWAS_ID_label_filter` to provide a file containing PheWAS_IDs of phenotypes that should be labelled.

---

#### Sig_pheno graph
Graphs with suffix **sig_pheno** are graphs showing only associations deemed significant (`--sig_FDR`). The Y axis is -log10(FDR) with a line of significance in red. The x-axis has individual phenotypes ordered by phenotype_group the lowest association within each group and then in descending order per group (only of significant results). Phenotypes are labelled using short_desc. The value in parenthesis is the category of phenotype seen in the tables, this also indicates whether a phenotype is male stratified (male), female stratified (female) or age of onset (age_of_onset). The direction of effect is signified by a triangle either pointing up or down. The phenotype_group is shown in the legend under Phenotype Category.

---

### Example 1 - PLINK results
Here we produce tables and graphs for the R association results, you can use any of the examples just replace `--results_file` argument, you may also wish to change the `--save_folder` or `--analysis_name` to save multiple sets of tables and graphs.

```bash
./05_tables_graphs.R \
--results_file $phewas_folder/analysis/example_plink_analysis/association_results/example_plink_association_results_list.gz \
--analysis_name example \
--plink_results \
--SNP_list $package_folder/extdata/worked_example/snp_info.gz \
--per_snp_graph \
--save_table_per_snp \
--save_folder $phewas_folder/tables_graphs/plink-standard/
```
This produces two tables, and two graphs. One table and both graphs are found here `$phewas_folder/tables_graphs/plink-standard/example_all_results_per_SNP/`, by looking at the graphs you should see two significant results, one for **standing height** and one for **pneumonia**, the coded allele is associated with decreased standing height and increased risk of pneumonia. The table `example_graph_all_results.csv` represents the results for this SNP, in cases where there is more than one SNP there would be more that one table produced (tables are always produced) and multiple graphs dependent on how many SNPs had at least one significant association. The table found here `$phewas_folder/tables_graphs/plink-standard/example_all_filtered_results.csv` contains the results for all the SNPs for a grouping, here the only grouping is **all** the default when no grouping file has been used in the analysis. 

---

### Example 2 - R association results
The code below will produce results for the CNV and GRS results produced earlier.

```bash
./05_tables_graphs.R \
--results_file $phewas_folder/analysis/example_CNV/CNV_example_all_group_results_list.RDS \
--analysis_name example_CNV \
--R_association_results \
--R_association_graph \
--save_folder $phewas_folder/analysis/tables_graphs/CNV 

./05_tables_graphs.R \
--results_file $phewas_folder/analysis/example_GRS/example/example_results_list.RDS \
--analysis_name example_GRS \
--R_association_results \
--R_association_graph \
--save_folder $phewas_folder/analysis/tables_graphs/GRS
```

If we look at the results there are no graphs and only a single table of results meaning there were no significant associations. If you still want to see the graph output regardless of any significant results use the `--save_all_graphs` argument, amending the code for the GRS results we can now produce a graph.

```bash
./05_tables_graphs.R \
--results_file $phewas_folder/analysis/example_GRS/example/example_results_list.RDS \
--analysis_name example_GRS \
--R_association_results \
--R_association_graph \
--save_folder $phewas_folder/tables_graphs/GRS \
--save_all_graphs
```

There are some circumstances where labelling a specific phenotype(s) can be useful to do this we need to use the argument `--PheWAS_ID_label_filter` and input the location of a plain text file with a single column labelled **PheWAS_ID** and list the PheWAS_IDs we wish to be labelled. Lets assume for this example that the GRS is related in some way to our SNP results from the PLINK analysis and that the association with pneumonia is of particular interest, running the code below reproduces the graph but now labels the pneumonia phenotype. 

```bash
./05_tables_graphs.R \
--results_file $phewas_folder/analysis/example_GRS/example/example_results_list.RDS \
--analysis_name example_GRS \
--R_association_results \
--R_association_graph \
--save_folder $phewas_folder/tables_graphs/GRS \
--save_all_graphs \
--PheWAS_ID_label_filter $package_folder/extdata/worked_example/PheWAS_ID_label_filter.gz
```

---


