# Calculation of the CPOM Density from the 2013 Survey

## Metadata

Code created 1 July 2015

Modified:

## Purpose

Calculate the density of the CPOM in the ponds from the survey conducted during 2013

## Description

The code in this file reanalyzes the data that was collected during the CPOM survey completed in 2013.  The documentation of the data are kind of a mess but the data file is in good shape, so this code is just starting over with that.

## Calculation

### Load data

    dens <- read.table("./data/CPOM_survey_2013_calc.txt", header = T)

#### Rewrite data as a .csv file

    write.table(dens, "./data/CPOM_survey_2013_calc.csv", row.names = F, quote = F, sep = ",")
