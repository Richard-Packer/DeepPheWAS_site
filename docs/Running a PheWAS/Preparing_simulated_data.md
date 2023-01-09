---
layout: default
title: Preparing to use simulated data
nav_order: 1
parent: Running a PheWAS
---

There are a few required and quality of life steps to run before starting to use the simulated data.

First open a BASH command line window and load the latest version of R available on your system:
`module load R/4.2.1`

Next locate your installation of DeepPheWAS or [install]({{ site.baseurl }}{% link index.md %}) DeepPheWAS in R now. Packages are by default installed in the home directory in folder called R  and sub folder that represents the R version being used, for our above R installation this would be.

`~/R/4.2/DeepPheWAS/`

As you can run all of the simulated data in a single interactive window we can define a variable `$package` and  that can be used throughout, use the code below replacing `~/R/4.2/DeepPheWAS` with the location of the R package on your system.

```bash
package=~/R/4.2/DeepPheWAS
```

Now locate a folder on your system to save all of the outputs of the PheWAS we will define this root folder as `$phewas_folder`, replace `/path/to/folder/` with the full path to folder.

```bash
phewas_folder=/path/to/folder/
```

We can now use `$package` and `$phewas_folder` throughout the remaining scripts using the simulated data.
