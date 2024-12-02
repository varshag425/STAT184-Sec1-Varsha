# STAT 184 Activity 12: Introduction to GitHub
This repository features the data analysis performed for STAT 184 Activity #10 - Creating a Quarto Document.

## Introduction
---
The aim of the data analysis for Activity 10 was to determine if gender played a significant role in a student's chance of admission into UC Berkley in the year 1973. The data analysis was performed using [Berkeley's 1973 Graduate Admissions Dataset](https://waf.cs.illinois.edu/discovery/berkeley.csv) and edited in Quarto Markdown. This dataset contains records of applicants, with details about the year and major they applied for, their gender and admission decision. The accompanying R code, provided as part of the assignment, guided the focus and direction of this analysis. This work is documented in the file STAT184_Activity10-1.qmd, which can be found in the 'code' folder of this repository.

## Implementation
---
This analysis features:
- Data Inventory : loading the dataset and other useful packages
    - ggplot2: used for data visualizations
    - dplyr: for data manipulation
    - tidyr: to create tidy data
- Exploratory Data Analysis: used head() and summary() funcitons to understand the structure of the dataset
- Data Anlyses and corresponding Visualizations
    - Admission Rates by Gender: We calculate the admission rates (calculated as a proportion) based on gender and compare the two in tabular and graphical form. This step helped to identify that male applicants approximately have a 0.45 chance of being accepted, whereas female applicants approximately have a 0.35 chance in being accepted.
    - Admission Rates by Major:
    - Applications Grouped by Gender and Major
    - Admission Rates Grouped by Gender and Major
    - Gender Difference in Admission Rates by Major



## Results and Conclusions
---

## Acknowledgements
---
Accompanying R code was provided as part of assignment 10.

## Contact Info
---
Please contact me at vpg5172@psu.edu with any questions/comments/concerns.
