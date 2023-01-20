---
title: Home
layout: home
nav_order: 1
---

DeepPheWAS is an R package for performing phenome wide association studies (PheWAS). It has been designed to give the user full control of the PheWAS process from phenotype generation through association testing and visualisation of the results. Unlike some R packages DeepPheWAS is not designed to be used interactively via IDEs such as Rstudio but via BASH command line arguments that are then passed to R scripts via an arguments parser ([docopt]) which run the R functions. In the genetics field this is not dissimilar to the [SAIGE] software for genetic association testing.

---

### Installation
The easiest way to install DeepPheWAS is via 
```r
devtools::install_github("Richard-Packer/DeepPheWAS")
```

DeepPheWAS is reliant on a number of packages and is being tested using the most up to date stable releases for those packages, it is therefore recommended to update the required packages if this option is provided upon installation.

To use all of the features of DeepPheWAS there must be [PLINK 2] installed, with the optional installation of [bgenix] adding more flexibility in extracting genetic data for association testing. 

---

### How to use
The majority of this site is dedicated to how to use DeepPheWAS by interacting with the included simulated data. We hope to cover the main features of the package and associated functions as well as the expected data input to output the expected phenotypes. Please work you way through the pages to get more familiar with the package and to get started on your own PheWAS projects.

[SAIGE]: https://saigegit.github.io/SAIGE-doc/
[docopt]: http://docopt.org/
[bgenix]: https://enkre.net/cgi-bin/code/bgen/doc/trunk/doc/wiki/bgenix.md
[PLINK 2]: https://www.cog-genomics.org/plink/2.0/
