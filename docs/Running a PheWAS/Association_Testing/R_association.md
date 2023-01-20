---
layout: default
title: R association
nav_order: 2
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

## 03b_R_association_testing.R

This script is an alternative to the `03a_PLINK_association_testing.R` script for running association analyses using R. While not streamlined for large-scale PheWAS, this script offers greater flexibility in the kind of analyses that can be run, such as fitting non-standard models or if the genetic data are not in a format readable by standard software. Further, this script allows for analyses of genomic risk scores (GRS), polygenic risk score (PRS), and multi-allelic copy number variants (CNV).

The script has slightly different inputs when testing risk scores vs non-risk scores (CNVs or similar).

**Risk score testing**

If testing GRS or PRS a guide must be provided as a csv file that acts to point the script to the correct data. A template `GRS_PRS_template.csv.gz` can be found here `$package_folder/extdata/GRS_PRS_template.csv.gz` and should be copied to another location to allow editing. The file contains five columns.
* **trait**: Name of the trait that the GRS/PRS has been created from.
* **group**: Grouping variable that the analysis is subset to, if no grouping has been used the default is **all**.
* **phenotype_data**: Full file path for the phenotype data that will be used in the association analysis created using `01_phenotype_preparation.R`.
* **genetic_data**: Full file path to the genetic data for the GRS/PRS.
* **column_name**: Column name in the genetic data file containing the relevant GRS genetic data.

GRS/PRS data is analysed per trait-group combination, results are saved as a R list object per trait that contain all trait-group combinations, this allows multiple traits to be analysed across multiple groups via a single input to the script.

**Non-GRS/PRS testing**

Non-GRS/PRS data is assumed to be an alternative genetic measurement with a column of IDs followed by 1-n columns of genetic data, example CNVs. Results are per column of the input file (minus ID column) with a single R object saved containing a list of results per-group, such that each non-ID column will be tested for association with all available phenotypes in each of the inputted groups. Unlike the GRS/PRS input the full location of all the phenotype files is required as input.

**Covariates**

If a covariate file is included then it must contain a column age, when selecting covariates note currently only participants with full covariate data will be analysed.

**Usage**
**Risk score testing**
```bash
03b_R_association_testing.R --analysis_folder=<folder> --GRS_input=<FILE> 
[--covariates=<FILE> --N_cores=<number> --PheWAS_manifest_overide=<FILE> 
--use_existing_results_file=<FILE>] 
[--phenotype_inclusion_file=<FILE> | --phenotype_exclusion_file=<FILE>]
```

**Non-risk score testing**
``` bash
03b_R_association_testing.R --analysis_folder=<folder> --non_GRS_data=<FILE> 
--phenotype_files=<FILE> --analysis_name=<name> 
[--covariates=<FILE> --group_name_overide=<text> --N_cores=<number> 
--PheWAS_manifest_overide=<FILE> --use_existing_results_file=<FILE>] 
[--phenotype_inclusion_file=<FILE> | --phenotype_exclusion_file=<FILE>]

```

For full details of all of the arguments please use the help argument `-h`. 

---

### Example - Risk score testing

For the example data we have provided a separate template for the `--GRS_input` found in `$package_folder/extdata/worked_example/GRS_guide` and produced simulated genetic data here `$package_folder/extdata/worked_example/GRS_example_data.gz`. First copy `$package_folder/extdata/worked_example/GRS_guide` to `$phewas_folder/data/GRS_guide`, edit the third column by selecting any of the complete phenotype tables found in `$phewas_folder/phenotype_table` and fourth column by adding in the full path to the `$package_folder` at the beginning of the existing text in the column. 

```bash
./03b_R_association_testing.R \
--analysis_folder $phewas_folder/analysis/example_GRS \
--GRS_input /$phewas_folder/data/GRS_guide \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

One R object is generated holding the results found here `/$phewas_folder/analysis/example_GRS/example/example_results_list.RDS`.

---

### Example - Non-risk score testing

Here we have used the initial example phenotype table created by the `01_phenotype_preparation.R`, feel free to change to any of the other examples provided.

```bash
./03b_R_association_testing.R \
--analysis_folder $phewas_folder/analysis/example_CNV \
--non_GRS_data $package_folder/extdata/worked_example/CNV_example.gz \
--analysis_name CNV_example \
--phenotype_files  $phewas_folder/phenotype_table/all_pop_example_table.gz \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

One R object is generated holding the results found here `/$phewas_folder/analysis/example_CNV/CNV_example_all_group_results_list.RDS`.

---
