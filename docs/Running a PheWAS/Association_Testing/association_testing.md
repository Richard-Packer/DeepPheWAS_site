---
layout: default
title: Association testing
nav_order: 4
parent: PheWAS scripts and examples
has_children: true
---
DeepPheWAS offers two methods for association testing that uses either:

* **PLINK 2** (`03a_PLINK_association_testing.R`) or 
* **R** (`03b_R_association_testing.R`) 

To perform regression against the available phenotypes. Covariates do no need to be included for either function to work although it is recommended to include covariates. Standard practice would be to include age, sex, array (if multiple used), and some number of principle components. 

The following sections detail the use of these two methods alongside the additional, `02_extracting_snps.R`, `04_combine_split.R` scripts that supplement the `03a_PLINK_association_testing.R` functions.
