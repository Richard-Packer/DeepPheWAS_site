---
layout: default
title: PLINK association testing
nav_order: 1
parent: Association testing
grand_parent: PheWAS scripts and examples
---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>
To use **PLINK 2** regression **PLINK 2** will need to be downloaded see [here] and made available on the users `$PATH`. 

PLINK can be used to test association with any genetic instrument that can be stored with a binary bed/pgen/bgen file. This is most commonly SNPs. To run association we first need to extract the variants of interest from the larger genetic files, we do this using the `02_extracting_snps.R` script. Once extracted we run the `03a_PLINK_association_testing.R` script. If we are testing a large number of variants and have access to distributed computing we split the phenotypes into to smaller chunks and analyse them separately, we achieve this again with `03a_PLINK_association_testing.R`, to combine the results we can use `04_combine_split.R`. We now go through each of these with example inputs.

---

## 02_extracting_snps.R

This script uses **bgenix** or **PLINK 2** to extract SNPs from genetic data, in .bgen, .bed or .pgen formats, using SNP identifiers and chromosome number. As such, bgenix and/or PLINK should be installed and available on the users `$PATH`.

First, the `genetic_file_guide_template.csv` file must be edited to provide the file location of the genetic files and corresponding .sample/.fam/.psam file for each of the 22 autosomal and 2 sex chromosomes. If genetic data for a particular chromosome or chromosome(s) are missing, the corresponding rows in the data/genetic_file_guide_template.csv file should be deleted. If the genetic data are not stored per chromosome, then different values can be used in the chromosome column. The genetic_file_guide_template.csv is found within the `$package_folder/extdata/genetic_file_guide.csv.gz` and should be copied to another location to allow editing, the three columns within the file are. 
* **chromosome**: Chromosome identifier, typically a number from 1 to 22 or the letters X and Y.
* **genetic_file_location**: Full path of the genetic data file for the corresponding chromosome.
* **psam_fam_sample_file_location**: Full path of the .sample, .fam or .pgen file, as necessary.

Second the SNP_list file must be edited a template for SNP_list can be found here `$package_folder/extdata/SNP_list_template.csv.gz`, once again this should be copied to another location to allow editing, it has five columns.
* **chromosome**: Chromosome identifier, typically a number from 1 to 22 or the letters X and Y. This should be in the same format as the `genetic_file_guide_template.csv` file. For example, not expressed as "01" or "chr1" if this is given as "1" in the above file.
* **rsid**: SNP identifier.
* **group_name**: Option to assign a common group name to SNPs that are related, such as being in the same credible set, graphs and tables can be produce that aggregate results across common group_names.
* **coded_allele**: Coded allele for the corresponding SNP.
* **non_coded_allele**: Non-coded allele for the corresponding SNP.
* **graph_save_name**: save name for the later graphing script.

**Usage**

```bash
02_extracting_snps.R (--genetic_file_guide=<FILE> --SNP_list=<FILE> --analysis_folder=<FOLDER>) 
(--plink_input | --bgen_input) [--plink_exe=<text> --bgenix_exe=<text> --plink_type=<text> --ref_bgen=<text> 
--variant_save_name=<name> --no_delete_temp]

```

For full details of all of the arguments please use the help argument `-h`. 

---

### Example - extraction

For the example data we have provided a separate template for the `--genetic_file_guide` found in `$package_folder/extdata/worked_example/genetic_file_guide.csv` and completed the `--SNP_list`. First copy `$package_folder/extdata/worked_example/genetic_file_guide.csv` to `$phewas_folder/data/genetic_file_guide.csv`, edit the second and third columns adding in the full path to the `$package_folder` at the beginning of the existing text in the column. 

```bash
./02_extracting_snps.R \
--genetic_file_guide $phewas_folder/data/genetic_file_guide.csv \
--SNP_list $package_folder/extdata/worked_example/snp_info.gz \
--analysis_folder $phewas_folder/analysis/ \
--plink_input \
--variant_save_name example_snps
```

The script will produce four files, `example_snps.log`, `example_snps.pgen`, `example_snps.pvar`, `example_snps.psam`, representing the values for a single simulated SNP for 100,000 simulated participants.

---

## 03a_PLINK_association_testing.R

Runs association analysis in using **PLINK 2**. Takes the variants extracted using `02_extracting_snps.R` and performs regression analysis on the available phenotypes per inputted group (optional).

Requires location of a folder where analysis will be hosted, a comma separated list of phenotype files derived from `01_phenotype_preparation.R`, an optional covariate file edited for use in plink (see below), and the location of the variants for association prepared with `02_extracting_snps.R`. By default, it will run association tests on all included phenotypes in the phenotype files used and use those phenotype files to assign group names, all analysis is then performed per group, with no groups the results use the suffix **"all"**. 

Phenotypes can be specified using one of two arguments `--phenotype_inclusion_file` or `--phenotype_exclusion_file`, group names can be specified using the `--group_name_overide` argument. Results are saved as an R object with option to save tables per-group of the combined raw plink results using the `--save_plink_tables` argument. 

With a large number of variants the regression analysis can take a long time, to make for more efficient analysis the phenotypes for any one or more of the analysed groups can be split using the `--split_group` input. When a group is split the phenotype files are split into smaller chunks, the analysis can then be performed in a cluster environment.To perform the analysis this same script can be used again but with the `--split_analysis` argument used to amend how results are saved. Results are then combined with `04_combine_split_analysis.R` script. If splitting the analysis, tables and figures should not be compiled until after `04_combine_split_analysis.R` is complete. 

**Covariate edit**

A tab-separated file that contains one row of data per individual to be analysed and one column for each covariate to be included in the association model. Must start with the columns ‘#FID’ and ‘IID’ both of which should be participants IDs, sex should not be included as this information is held with the PLINK files.

**Usage**

```bash
03a_PLINK_association_testing.R  (--analysis_folder=<FOLDER> --phenotype_files=<FILE> --variants_for_association=<FILE> 
--analysis_name=<name>) [--covariate=<file> --group_name_overide=<text>] [--split_analysis --PheWAS_manifest_overide=<FILE> 
--plink_exe=<command> --save_plink_tables --split_group=<name> --N_quant_split=<number> --N_binary_split=<number> --model=<text> 
--check_existing_results] [--phenotype_inclusion_file=<FILE>  | --phenotype_exclusion_file=<FILE>]

```

For full details of all of the arguments please use the help argument `-h`. 

---

### Example 1 - standard analysis

The code below uses the sex_stratified and age of onset phenotypes created in example 3 of [Phenotype preparation]({% link docs/Running a PheWAS/phenotype_prep.md %}). No covariate is included but is strongly recommended when performing PheWAS on real data.

```bash
./03a_PLINK_association_testing.R \
--analysis_folder $phewas_folder/analysis/example_plink_analysis/ \
--phenotype_files $phewas_folder/phenotype_table/all_pop_example_table_sex_aoo.gz \
--variants_for_association $phewas_folder/analysis/example_snps.pgen \
--analysis_name example_plink \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

The script will produce a folder `$phewas_folder/analysis/example_plink_analysis/` to contain all the results, raw plink results are stored in `$phewas_folder/analysis/example_plink_analysis/plink`, association results are stored as an R object and found here `$phewas_folder/analysis/example_plink_analysis/association_results/example_plink_association_results_list.gz`.

---

### Example 2 - split analysis
Here we will show how to split the phenotypes for analysis and then analyse the split files using the same `03a_PLINK_association_testing.R` script.

First split the phenotypes, as we have not used a grouping variable in the [Phenotype preparation]({% link docs/Running a PheWAS/phenotype_prep.md %}) stage, the default grouping variable **all** is used as the `--split_group` argument, we have also amended the `--analysis_folder` to save to a new location.

```bash
./03a_PLINK_association_testing.R \
--analysis_folder $phewas_folder/analysis/example_plink_analysis_split/ \
--phenotype_files $phewas_folder/phenotype_table/all_pop_example_table_sex_aoo.gz \
--variants_for_association $phewas_folder/analysis/example_snps.pgen \
--analysis_name example_plink \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz \
--split_group all
```

This splits the phenotype table into two files. The number of files will depend on the number of phenotypes and the arguments `--N_quant_split` and `--N_binary_split`, in this case the example data has only 25 phenotypes, one file has been created for the binary phenotypes and one for the quantitative phenotypes. These phenotype files are stored here `$phewas_folder/analysis/example_plink_analysis/split_all`. An additional file `$phewas_folder/analysis/example_plink_analysis/all_split_guide` is also created, this can be used as a guide when using distributed computing, we will not use it here.

---

Next we analyse the split phenotypes again using `03a_PLINK_association_testing.R` adding the `--split_analysis` argument, removing `--split_group`  and changing the `--phenotype_files` to one of the newly created split phenotype files. This needs to be done twice once for each of the split phenotype files. When using distributed computing each of these analysis would occur independently at the same time. 

```bash
./03a_PLINK_association_testing.R \
--analysis_folder $phewas_folder/analysis/example_plink_analysis_split/ \
--phenotype_files $phewas_folder/analysis/example_plink_analysis_split/split_all/all_1 \
--variants_for_association $phewas_folder/analysis/example_snps.pgen \
--analysis_name example_plink \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz \
--split_analysis

./03a_PLINK_association_testing.R \
--analysis_folder $phewas_folder/analysis/example_plink_analysis_split/ \
--phenotype_files $phewas_folder/analysis/example_plink_analysis_split/split_all/all_2 \
--variants_for_association $phewas_folder/analysis/example_snps.pgen \
--analysis_name example_plink \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz \
--split_analysis
```
Split analyses works by saving only the raw PLINK files, to combine these into a single result we now use the `04_combine_split.R` script.

---

## 04_combine_split.R
This script combines the results from the `--split_group` and `--split_analysis` arguments from `03a_PLINK_association_testing.R` into a single result file and appends the original results file first removing the empty list and then adding the split group into it.

**Usage**

```bash
04_combine_split.R (--split_plink_results_folder=<folder>  --results_RDS_file=<FILE> 
--group_name=<name> --analysis_name=<name>)
```

For full details of all of the arguments please use the help argument `-h`. 

---

### Example - split combine
The script combines the plink results found in the `--split_plink_results_folder` only for the group that was split, in this case there were no groups and so the default `--group_name` **all** is used. Even though no results were produced using the initial `03a_PLINK_association_testing.R` as the all phenotypes were initially split, a `--results_RDS_file` is always produced, in this case it was empty. In cases where it is not empty `04_combine_split.R` only updates this results file with the results from the spilt group.

``` bash
./04_combine_split.R \
--split_plink_results_folder $phewas_folder/analysis/example_plink_analysis_split/plink_results \
--results_RDS_file $phewas_folder/analysis/example_plink_analysis_split/association_results/example_plink_association_results_list.gz \
--group_name all \
--analysis_name example_plink
```

---

[here]:https://www.cog-genomics.org/plink/2.0/
