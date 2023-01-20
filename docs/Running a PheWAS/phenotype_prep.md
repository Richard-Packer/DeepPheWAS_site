---
layout: default
title: Phenotype preparation
nav_order: 3
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

You should now be at the stage where all of the phenotypes have been generated and are stored as R data objects. The following script converts these objects into tables, converting binary phenotypes into cases, controls and exclusions and (optionally) transforming quantitative phenotypes using inverse normal transformation. It is at this stage where you can add the optional sex stratified phenotypes and age of onset phenotypes (only in those phenotypes that are not already stratified by sex and have reliable date of onset respectively).

To run all scripts after this point first run.

```bash
cd $package_folder/extdata/scripts/association_testing/
```
---

## 01_phenotype_preparation.R

**Usage**

```bash
01_phenotype_preparation.R (--phenotype_folder=<FOLDER> | --phenotype_files=<FILES>) 
(--phenotype_filtered_save_name=<FILE>) [(--relate_remove --kinship_file=<FILE>)] 
[(--sex_split_phenotypes=<FILE> --sex_info=<FILE> | --sex_split_all --sex_info=<FILE>)] 
[(--age_of_onset_phenotypes=<FILE> --DOB_file=<FILE> | --age_of_onset_all --DOB_file=<FILE>)] 
[--groupings=<FILE> --quantitative_Case_N=<number> --binary_Case_N=<number> --male=<number> 
--female=<number> --PheWAS_manifest_overide=<FILE> --IVNT --save_RDS=<FILE> --stats_save=<FILE>]

```

There are appreciable more inputs for this script than those used for generating phenotypes, this reflects the increased number of options available for the user. We will go through several of these within the examples, but for full details of all of the arguments please use the help argument `-h`. 

### Example 1 - standard input

Below is the standard input for preparing phenotypes for association, here the phenotypes are first filtered by case number (default values 50 and 100 used if not added as arguments `--quantitative_Case_N=<number> --binary_Case_N=<number>` respectively) and then filtered by relatedness and then filtered again by case number (in case removing individuals who are related has dropped the case number below the threshold for inclusion). The inverse normal transformation is applied to all quantitative traits using the `--IVNT` argument. The optional argument `--groupings` has not been used in the example but would be commonly used in real PheWAS to subset the population, usually by ancestry. If `--grouping` is used then the script performs all filtering steps per grouping and produces and table of phenotypes per group. The scripts also produces two stats files (when using `--stats_save` argument), which will record the case and control numbers per phenotype for all groupings after initial filtering and after related individuals have been removed.

```bash
./01_phenotype_preparation.R \
--phenotype_filtered_save_name $phewas_folder/phenotype_table/example_table \
--phenotype_folder $phewas_folder/data/phenotypes/ \
--relate_remove \
--kinship_file $phewas_folder/data/related_callrate \
--IVNT \
--stats_save $phewas_folder/phenotype_table/example_stats
```
As no `--groupings` file is provided this saves a single phenotype table `all_pop_example_table.gz` that uses the default group name **"all"** as a suffix, and two stats files `example_stats_N_filtered.csv` and `example_stats_relate_remove.csv`

---

### Example 2 - sex stratified phenotypes

Below we have added the `--sex_split_phenotypes` and the `--sex_info` arguments (both are required). The `--sex_info` is taken from the `combined_sex` file that was created by the `02_data_preparation.R script`. The `--sex_split_phenotypes` requires a file with the header **PheWAS_ID** with each phenotype that requires a sex stratified version specified. Another option is to use the `--sex_split_all` argument which will create sex stratified phenotypes for all of the existing non-sex specific phenotypes available. Try replacing the `-sex_split_phenotypes` with `--sex_split_all`, remember to change the save name of the `--phenotype_filtered_save_name` argument and  `--stats_save` if you want to have both versions available.

```bash
./01_phenotype_preparation.R \
--phenotype_filtered_save_name $phewas_folder/phenotype_table/example_table_sex \
--phenotype_folder $phewas_folder/data/phenotypes/ \
--relate_remove \
--kinship_file $phewas_folder/data/related_callrate \
--IVNT \
--stats_save $phewas_folder/phenotype_table/example_stats_sex \
--sex_split_phenotypes $package_folder/extdata/worked_example/sex_filter.txt.gz \
--sex_info $phewas_folder/data/combined_sex
```

---

### Example 3 - age of onset phenotypes

Below we have added the `--age_of_onset_phenotypes` and the `--DOB_file` arguments (both are required). The `--DOB_file` is taken from the `DOB` file that was created by the `02_data_preparation.R script`. The `--age_of_onset_phenotypes` requires a file containing four columns **PheWAS_ID**,**lower_limit**,**upper_limit** and **transformation**. 

**PheWAS_ID** are the phenotypes to create the age_of_onset phenotypes for, **lower_limit** is the lower age boundary to filter read as <=, **upper_limit** is the upper age boundary acceptable read as >=. **Transformation** is the type of transformation (if any) that should be applied to the phenotype. Current the only accepted transformation is "**IVNT**" for inverse normal transformation, if no transformation is required input **NA**. All age of onset phenotypes to be labelled **(PheWAS_ID)_age_of_onset**. Unlike the equivalent sex stratified `--sex_split_all` argument the `--age_of_onset_all` argument is not quite stable for use and should not be inputted. 

Not all phenotypes can produce a useful age_of_onset variant, quantitative phenotypes for example have no meaningful age of onset, so some thought should be taken when selecting phenotypes when generating their age of onset variant.

```bash
./01_phenotype_preparation.R \
--phenotype_filtered_save_name $phewas_folder/phenotype_table/example_table_sex_aoo \
--phenotype_folder $phewas_folder/data/phenotypes/ \
--relate_remove \
--kinship_file $phewas_folder/data/related_callrate \
--IVNT \
--stats_save $phewas_folder/phenotype_table/example_stats_sex_aoo \
--sex_split_phenotypes $package_folder/extdata/worked_example/sex_filter.txt.gz \
--sex_info $phewas_folder/data/combined_sex \
--age_of_onset_phenotypes $package_folder/extdata/worked_example/aoo_filter.txt.gz \
--DOB_file $phewas_folder/data/DOB
```

---



