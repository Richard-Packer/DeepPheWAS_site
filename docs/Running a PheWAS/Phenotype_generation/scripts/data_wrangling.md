---
layout: default
title: Data wrangling
nav_order: 1
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

### 01_minimum_data.R

This script combines data-field UK Biobank data into a single file that is used throughout by DeepPheWAS functions, throughout the remaining scripts this file is referred to as the **“min_data”** file. This is achieved by sub-setting the aforementioned data to columns of interest and, where the data are spread across multiple files, merging across all files provided. For UK Biobank data, the column names are referenced by data-field IDs. The desired subset of columns to be extracted is listed in the data_field_ID file found, by default, in `$package_folder/extdata/fields-minimum.txt.gz`. This file can be edited to extract a different subset of columns to that provided by default. Files can be inputted in a single folder using the `--data_folder` flag, or specified individually using the `--data-files` flag.

**Usage**

```bash
01_minimum_data.R (--data_folder=<FOLDER> | --data_files=<FILES>) (--save_loc=<FILE>) 
[--r_format --data_field_ID=<FILE> --data_name_pattern=<text> --N_cores=<number> --exclusions=<FILE>]
```

For full details of all of the arguments please use the help argument `-h`. 

**Example**

Assuming you have loaded R and inputted your own folder locations for the `$package` and `$phewas_folder` variables, the below code will generate a table of data-fields from the simulated data inputs.

```bash
cd $package_folder/extdata/scripts/phenotype_generation/

./01_minimum_data.R \
--data_file $package_folder/extdata/worked_example/ukb_data.csv.gz \
--save_loc $phewas_folder/data/min_tab_test.gz
```

The file that is produced `min_tab_test.gz` will have the same format as that created when using real data and should be the file inputted when for the `--min_data` argument is required.

---

### 02_data_preparation.R
This script concatenates and edits linked healthcare data from UK Biobank into a single long file with participant identifier, clinical code, date of clinical code’s recording, and data source. Current data sources are:

* **“MD”** – when ICD-10 code is taken from mortality data.
* **"cancer"** – when ICD-10 code is taken from cancer registry data.
* **"ICD10-1"** – when an ICD-10 code is taken from hospital records as one of the primary causes of admission.
* **"ICD10-2"** – when an ICD-10 code is taken from hospital records as a secondary cause of admission.
* **"ICD10-3"** – when an ICD-10 code is taken from hospital records as an external cause of admission.
* **"ICD9-1"** – when an ICD-9 code is taken from hospital records as one of the primary causes of admission.
* **"ICD9-2"** – when an ICD-9 code is taken from hospital records as an external cause of admission.
* **"ICD9-3"** – when an ICD-9 code is taken from hospital records as one of the primary causes of admission.
* **"OPCS"** – A OPCS-4 code taken from the hospital records.
* **"SR"** – A self-report code for a non-cancer illness taken from data field 20002.
* **"SROP"** - A self-report code for an operation taken from data field 20004.
* **"V2"** – A Read V2 code taken from the primary care clinical data.
* **"V3"** – A Read V3 code taken from the primary care clinical data.

The current iteration of this script is purpose-built for UK Biobank data. As such, if this script is to be applied to non-UK Biobank data, it would first need to be formatted to mimic that of UK Biobank or to mimic the expected output of the script. The two self-reported data sources are specific to UK Biobank and if similar data were available in another data set, these would need to be mapped to the UK Biobank codes used in data fields 20002 and 20004. 
Primary care prescription data from UK Biobank does not lend itself to be easily combined in this way and so it is edited and saved as a separate file.

**Usage**

```bash
02_data_preparation.R (--save_location=<FOLDER>) [--min_data=<FILE> --GPC=<FILE> --GPP=<FILE> --hesin_diag=<FILE> 
--HESIN=<FILE> --hesin_oper=<FILE> --death_cause=<FILE> --death=<FILE> --exclusions=<FILE> --king_coef=<FILE>]
```

**Example**

```bash
./02_data_preparation.R \
--save_location $phewas_folder/data/ \
--min_data $phewas_folder/data/min_tab_test.gz \
--hesin_diag $package_folder/extdata/worked_example/HES_Diag.csv.gz \
--HESIN $package_folder/extdata/worked_example/hesin.csv.gz \
--death_cause $package_folder/extdata/worked_example/death.csv.gz \
--death $package_folder/extdata/worked_example/death_date.csv.gz \
--king_coef $package_folder/extdata/worked_example/KING_coef.csv.gz \
--GPC $package_folder/extdata/worked_example/GPC.csv.gz
```

The `health_records.txt.gz`, `DOB`, `control_populations` and `related_callrate` files that are produced in this example are identical in format to those produced by real data. If the user had non UK Biobank data then replicating these files who provide almost all the healthcare data and would allow the majority of phenotypes to be generated.

---

