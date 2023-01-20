---
layout: default
title: Running on real data
nav_order: 4
---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Allocating the appropriate time and memory on a distributed computing system will always depend on the exact specification of the system in place, we have developed a rough guide to both expected memory requirement and output file size to assist using DeepPheWAS on real datasets. This is likely an overestimate for each script, and should be refined on your own system, using the `--N_cores` flag has the effect of increasing memory requirements substantially in some scripts

Using the UK Biobank dataset or one of similar size (data for 500,000 participants).

---

## Phenotype Preparation

### 01_minimum_data.R

**Running script**

* **80 GB** memory
* **2 hour** run time limit

**Output Files**

* **min_data** ~800MB

---

### 02_data_preparation

**Running script**

* **80 GB** memory
* **2 hour** run time limit

**Output Files**

* **GP_C_edit.txt.gz** ~700MB
* **health_records.txt.gz** ~633MB
* **GP_P_edit.txt.gz** ~420MB
* **DOB** ~9.5MB
* **Control populations** ~5.8MB
* **combined_sex** ~5MB
* **related_callrate** ~1.7MB
* **GP_C_ID.txt.gz** ~1MB
* **control_exclusions** ~10kB

---

### 03a_phecode_generation.R

**Running script**

* **200 GB** memory
* **6 hour** run time limit

**Output Files**

* **phecodes.RDS** ~2.4GB
* **range_ID.RDS** ~13MB

---

### 03b_data_field_phenotypes.R

**Running script**

* **200 GB** memory
* **6 hour** run time limit

**Output Files**

* **data_field_phenotypes.RDS** ~670MB

---

### 03c_creating_concepts.R

**Running script**

* **120 GB** memory
* **6 hour** run time limit

**Output Files**

* **all_dates.RDS** ~28MB
* **concepts.RDS** ~16MB

---

### 03d_primary_care_quantitative_phenotypes.R

**Running script**

* **20 GB** memory
* **1 hour** run time limit

**Output Files**

* **PQP.RDS** ~4MB

---

### 04_formula_phenotypes.R

**Running script**

* **20 GB** memory
* **1 hour** run time limit

**Output Files**

* **formula_phenotypes.RDS** ~17MB

---

### 05_composite_phenotypes.R

**Running script**

* **60 GB** memory
* **3 hour** run time limit

**Output Files**

* **composite_phenotypes.RDS** ~81MB

---

### 03c_creating_concepts.R

**Running script**

* **120 GB** memory
* **6 hour** run time limit

**Output Files**

* **all_dates.RDS** ~28MB
* **concepts.RDS** ~16MB

---

## Phenotype preparation association analysis and tables/graphs

### 01_phenotype_preparation.R

**Running script**

* **200 GB** memory
* **12 hour** run time limit

**Output Files**

* **phenotype_tables** ~600MB for larger population groupings
* **stats_N_filtered** ~200kB
* **stats_relate_remove** ~200kB

---

### 02_extracting_snps.R

**Running script**

Can normally be run interactively on a BASH command line window.

**Output Files**

* **.pgen** ~varies based on number of SNPs, likely <50MB
* **.psam** ~8MB
* **.pvar** ~10kB

---

### 03a_PLINK_association_testing.R

**Running script**

Memory and run time varies significantly dependent on the number of SNPs, anything >300 SNPs consider splitting.

**Output Files** per trait

* **plink files** ~20MB
* **results RDS file** ~10MB


---

### 03b_R_association_testing

**Running script** Per trait

* **128 GB** memory
* **3 hour** run time limit 

**Output Files** per trait

* **results RDS file** ~200kB

---

### 05_tables_graphs.R

**Running script**

Can normally be run interactively on a BASH command line window.

**Output Files**

* Depends on the number of SNPs/GRS graphs and individual SNP or GRS tables are ~200kB

---


