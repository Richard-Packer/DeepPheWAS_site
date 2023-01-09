---
layout: default
title: Running a PheWAS
nav_order: 3
has_children: true
---


DeepPheWAS is an R package designed to be used via the inbuilt R-scripts. These R-scripts are accessed via the command line using a bash interface and utilise an argument parser (docopt) to process command line inputs (arguments). Docopt requires precise inputs to work and will not give any error message to highlight which input has been inputted incorrectly. It is recommended you read the documentation held on this website before attempting to run the scripts. 

There are a total of **14** **scripts** currently included with DeepPheWAS split into two folders.  
1. `R_x/DeepPheWAS/extdata/scripts/association_testing`
1. `R_x/DeepPheWAS/extdata/scripts/association_testing`

Within each folder the scripts are numbered in the order they should be run, those with an equal number such 03a, 03b etc can be run at the same time. Some scripts are optional dependent on the exact analysis you are wanting to run and the available data.
To see help and information on each of the scripts use a single argument `-h` for example.

`./01_minimum_data.R -h`

Will present the help documentation for the `01_minimum_data.R`. 

In the following sections we will go through the standard inputs to the scripts required to generate the phenotypes, prepare the phenotypes for association, run association testing and graph the results. We have provided simulated data in a compatible format to that provided by UK Biobank. All of the scripts using simulated data can be run interactively in a BASH window.
