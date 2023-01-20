---
layout: default
title: Phenotype creation
nav_order: 2
parent: Phenotype generation
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

---

### 03a_phecode_generation.R

This script maps healthcare data from the ICD-9 and ICD-10 coding frameworks to [Phecodes] and generates output that describes the resulting phenotypes in all individuals. In addition, output can be generated that describes individuals who should be excluded when selecting controls (binary phenotypes). For example, if one were interested in defining an asthma phenotype but wanted any controls to not have chronic obstructive pulmonary disease codes, that can be done using this script.

**Usage**

```bash
03a_phecode_generation.R (--health_data=<FILE> --sex_info=<FILE>) (--phecode_save_file=<FILE> | --no_phecodes) 
(--no_range_ID | --range_ID_save_file=<FILE>) [--control_exclusions=<FILE> --N_cores=<number> --ICD10=<text_comma> 
--ICD9=<text_comma> --PheWAS_manifest_overide=<FILE>]
```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

```bash
./03a_phecode_generation.R \
--health_data $phewas_folder/data/health_records.txt.gz \
--sex_info $phewas_folder/data/combined_sex \
--phecode_save_file $phewas_folder/data/phenotypes/phecodes.RDS \
--range_ID_save_file $phewas_folder/data/phenotypes/range_ID.RDS \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

The two R objects that are produced `phecodes.RDS` and `range_ID.RDS` are phenotype list objects, `phecodes.RDS` are phecode phenotypes and `range_ID.RDS` are lists of IDs that can be excluded as controls and are used in the composite phenotypes.

---

### 03b_data_field_phenotypes.R

This script creates Data-Field phenotypes from UK Biobank Data-Field IDs. The script uses the `PheWAS_manifest.csv.gz` file (located in the `$package_folder/extdata/PheWAS_manifest.csv.gz`) as a guide to creating phenotypes and will only create phenotypes where the required data-field ID data is available. If data from outside UK Biobank are being used, UK Biobank data-field IDs would need to be mapped to their equivalent in the alternative data source.

**Usage**

```bash
03b_data_field_phenotypes.R (--min_data=<FILE> --phenotype_save_file=<FILE>) 
[--N_cores=<number> --PheWAS_manifest_overide=<FILE> --append_file=<FILE>]
```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

```bash
./03b_data_field_phenotypes.R \
--min_data $phewas_folder/data/min_tab_test.gz \
--phenotype_save_file $phewas_folder/data/phenotypes/data_field_phenotypes.RDS \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

The file that is produced `data_field_phenotypes.RDS` is a phenotype list object.

---

### 03c_creating_concepts.R

This script uses code lists provided to define concepts. Concepts can then be combined to form cases and controls for "composite-phenotypes". A concept is a single homogeneous group of codes that all describe a single disease, symptom, or related group of medicines. 

The main output that is used for forming phenotypes is saved as an R object and is a record of the ID, number of codes and the date of the earliest code in a record. 

UK Biobank prescription data is unusual in that it does not contain consistent codes (namely BNF codes) for most of the data. As such, much of the searching is done using key search terms. A small subsection requires using Read version 2 drug codes. Because of this major difference with the other types of data, prescription concepts have their own function and would need to be used with caution in a non-UK Biobank setting.
Users can choose to define only concepts that use clinical data or prescription data or both. At least one must be inputted, however.

**Usage**

```bash
# can use either but allows user to only have health data and does not require prescription data.

# for prescription concepts
03c_creating_concepts.R (--GPP=<FILE> --concept_save_file=<FILE> --all_dates_save_file=<FILE>) 
[--health_data=<FILE>  --PheWAS_manifest_overide=<FILE> --code_list_folder_override=<FOLDER>]

# for all other concepts
03c_creating_concepts.R (--health_data=<FILE> --concept_save_file=<FILE> --all_dates_save_file=<FILE>) 
[--GPP=<FILE> --PheWAS_manifest_overide=<FILE> --code_list_folder_override=<FOLDER>]

```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

We have not provided simulated prescription data in our example and so do not need the `--GPP` argument.

```bash
./03c_creating_concepts.R \
--concept_save_file $phewas_folder/data/phenotypes/concepts.RDS \
--all_dates_save_file $phewas_folder/data/phenotypes/all_dates.RDS \
--health_data $phewas_folder/data/health_records.txt.gz \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

The file that is produced `concepts.RDS` and `all_dates.RDS` are a phenotype list objects.

---

### 03d_primary_care_quantitative_phenotypes.R

This script creates quantitative phenotypes from primary care data. The script uses the `PheWAS_manifest.csv.gz` file (located in the `$package_folder/extdata/PheWAS_manifest.csv.gz`) as a guide to creating phenotypes. 

**Usage**

```bash
 03d_primary_care_quantitative_phenotypes.R (--GPC=<FILE> --DOB=<FILE> --phenotype_save_file=<FILE>) 
[--N_cores=<number> --PheWAS_manifest_overide=<FILE> --code_list_overide=<FOLDER>]
```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

```bash
./03d_primary_care_quantitative_phenotypes.R \
--GPC $phewas_folder/data/GP_C_edit.txt.gz \
--DOB $phewas_folder/data/DOB \
--phenotype_save_file $phewas_folder/data/phenotypes/PQP.RDS \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

The file that is produced `PQP.RDS` is a phenotype list object.

---

### 04_formula_phenotypes.R

This script creates formula phenotypes from the phenotype data generated by earlier scripts. The script uses the `PheWAS_manifest.csv.gz` file (located in the `$package_folder/extdata/PheWAS_manifest.csv.gz`) as a guide to creating phenotypes. 

**Usage**

```bash
04_formula_phenotypes.R (--min_data=<FILE> --phenotype_save_file=<FILE>) [--data_field_phenotypes=<FILE> --sex_info=<FILE> 
--PheWAS_manifest_overide=<FILE>]
```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

```bash
./04_formula_phenotypes.R \
--min_data $phewas_folder/data/min_tab_test.gz \
--data_field_phenotypes $phewas_folder/data/phenotypes/data_field_phenotypes.RDS \
--sex_info $phewas_folder/data/combined_sex \
--phenotype_save_file $phewas_folder/data/phenotypes/formula_phenotypes.RDS \
--PheWAS_manifest_overide $package_folder/extdata/worked_example/example_manifest.gz
```

The file that is produced `data_field_phenotypes.RDS` is a phenotype list object.

---

### 05_composite_phenotypes.R

This script creates composite phenotypes and composite concepts using the `composite_phenotype_map.csv.gz` (located in the `$package_folder/extdata/composite_phenotype_map.csv.gz`) to guide the various functions. The phenotype creating script iterates over itself (default five times) as some composite phenotypes use other composite phenotypes in their definition.  

**Usage**

```bash
05_composite_phenotypes.R (--phenotype_save_file=<FILE>) (--phenotype_folder=<FOLDER> | --phenotype_files=<FILES>)
[--control_populations=<FILE> --N_iterations=<number> --update_list=<FILE> --composite_phenotype_map_overide=<FILE>]
```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

```bash
./05_composite_phenotypes.R \
--phenotype_folder $phewas_folder/data/phenotypes/ \
--phenotype_save_file $phewas_folder/data/phenotypes/composite_phenotypes.RDS \
--control_populations $phewas_folder/data/control_populations
```

The file that is produced `composite_phenotypes.RDS` is a phenotype list object.

---

**Congratulations** you have now generated all the required phenotypes for association testing.

[Phecodes]:https://phewascatalog.org/phecodes_icd10

