---
layout: default
title: Data
nav_order: 2
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>


DeepPheWAS is optimised for UK Biobank data, and assuming all required fields are present will make all phenotypes natively available with the package. DeepPheWAS can work with other data sources particularly other sources of health care records but requires some editing of the data that is not required when using UK Biobank data.

Currently DeepPheWAS only works with downloaded data files and not the research access portal (RAP) from UK Biobank, work is under way to convert the package for use in the RAP.

### Required data fields from UK Biobank
For those unfamiliar with [UK Biobank], data is accessed by application to be an approved researcher and requesting data in a "basket". Data is separated into categories that are then separated into fields, each field has an ID called a "Field ID". Most field IDs that are available for selection will be provided as a flat text/csv file that will contain participant ID for each row and the field ID for each column, with each selected field ID being combined into a single large table. There is a minimum set of field IDs required for DeepPheWAS to function, these are field IDs.

* **53** - Age at assessment centre
* **31** - Sex
* **52** - month of birth
* **34** - year of birth
* **22001** - genetic sex
* **22005** - missingness

Having only these fields will not create any phenotypes but allows the addition of date and sex data when creating phenotypes which is essential for DeepPheWAS to work. A full list of field IDs that are used by the package and accessed in this way is stored as a plain text file within the downloaded package. Find this file in the installed package `R_x/DeepPheWAS/extdata/fields_minimum.txt.gz`. The majority of these data fields will provide data for a single phenotype the most frequently used field IDs are.

* **20002** - Self-reported non-cancer diagnosis
* **20008** - Year of self-reported non-cancer diagnosis
* **20004** - Self-reported operation code 
* **20010** - Year of self-reported operation
* **40006** - Cancer diagnosis (cancer registry)
* **40005** - Date of cancer diagnosis

In addition to the data described above some linked health care data is provided as separate files available via the **"data portal"**, field IDs listed below alongside the name inputted in the data portal to retrieve the data file:
* **42040** - GP clinical event records (gp_clinical)
* **42039** - GP prescription records (gp_scripts)
* **41259** - Records in HES inpatient main dataset (HESIN)
* **41234** - Records in HES inpatient diagnoses dataset (HESIN_DIAG)
* **41149** - Records in HES inpatient operations dataset (HESIN_OPER)
* **40023** - Death cause clinical codes (death_cause)
* **40023** - Death cause dates (death)

Finally if a user wanted to remove related individuals from association testing they would need to use the [gfetch tool] made available by UK Biobank to downloaded the **rel** file containing information on related pairs
If a user had only the 

Having the above data and no other additional field IDs would create all phenotypes that rely solely on health data, but miss nearly all quantitative phenotypes and phenotypes that rely on questionnaire data.

### Non UK Biobank data 



[UK Biobank]: https://www.ukbiobank.ac.uk/enable-your-research/apply-for-access
[gfetch tool]: https://biobank.ctsu.ox.ac.uk/crystal/refer.cgi?id=668
