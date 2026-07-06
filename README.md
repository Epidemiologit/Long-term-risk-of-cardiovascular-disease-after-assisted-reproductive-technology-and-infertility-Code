# Long-term Risk of Cardiovascular Disease After Assisted Reproductive Technology and Infertility: a cohort study

## Overview
This repository contains the SAS scripts and R code used to select the study population for this cohort study, perform statistical analyses, generate tables and figures, and run quality checks. 

For a proper replication of the analysis pipeline, scripts should be executed in chronological order from `01` to `10`.

## Software Requirements
* **SAS** version 9.4
* **R** version 3.4.1

## File Structure & Pipeline

* **01_Database management**  
  Extracts the raw registry data, cleans, filters, and creates the final study cohort file containing exposure and outcome variables.
  
* **02_Figure_1**  
  Outputs Figure 1 (the study flowchart) for the manuscript. This file is completely indipendent and relies only on the packages the the numbers calculated in the file 01_Database management
  
* **03_Table_1**  
  Performs descriptive statistical analyses used to populate Table 1 in the manuscript.
  
* **04_Multiple_imputation_and_subsetting**  
  Takes the data file produced by script 01, performs multiple imputation for missing variables, runs imputation quality checks, and subsets the data into three separate files based on the comparison groups:
  * *Group 1:* ART vs. Infertility (Main analysis)
  * *Group 2:* Infertility vs. No Infertility (Secondary analysis)
  * *Group 3:* ICSI vs. Infertility (Supplemental analysis)
  
* **05_IPW_modelling**  
  Takes the three multiply-imputed data files from script 04 and generates four distinct sets of Inverse Probability Weights (IPW):
  * ART vs. Infertility (Fully adjusted, Main analysis)
  * Infertility vs. No Infertility (Fully adjusted, Secondary analysis)
  * Infertility vs. No Infertility (Age-adjusted, Supplement)
  * ICSI vs. Infertility (Fully adjusted, Supplement)
  
* **06_IPW_checks_S3_to_S4**  
  Performs quality checks on the weights and produces Supplementary Figure S3 (distribution of full vs. winsorized IPW) and Supplementary Figure S4 (Mirror graph).
  
* **07_SMD_S3_table**  
  Calculates the Standardized Mean Difference for unweighted, full IPW-weighted, and winsorized IPW-weighted variables to evaluate weighting performance. Statistics are exported to Excel to populate Table S3 in the Supplement.
  
* **08_Survival_analysis**  
  Performs the core survival analyses and exports the resulting statistics to an Excel file to populate Supplemental Tables S5 to S7.
  
* **09_Figures_2_to_3_and_S5_to_S7**  
  Applies Rubin's rules to pool the multiply-imputed datasets, then creates and assembles multi-panel graphs for Figures 2 and 3 in the main manuscript, as well as Figures S5 to S7 in the Supplement.
  
* **10_ICSI_Fig_S8**  
  Applies Rubin's rules to pool the multiply-imputed datasets and assembles the multi-panel graph for the ICSI vs. Infertility comparison (Supplementary Figure S8).

## R Packages Used
This project relies on a comprehensive suite of R packages categorized by their role in the analysis pipeline:

### Data Management & Core Utilities
* **tidyverse** (includes **dplyr**, **tidyr**, **purrr**, **forcats**) - For tidy data manipulation and workflow management.
* **haven** - For importing/exporting SAS datasets.
* **writexl** - For exporting final data frames to Excel formats.

### Multiple Imputation
* **mice** / **miceadds** / **micemd** - For handling missing data via Multivariate Imputation by Chained Equations.
* **lattice** - For multivariate data visualization during imputation diagnostics.
* **future** - For parallel processing to speed up computation times.

### IPW & Balance Checks
* **WeightIt** - For estimating propensity scores and generating inverse probability weights.
* **cobalt** - For assessing covariate balance and calculating standardized mean differences.
* **splines** - For modeling non-linear relationships in the weighting process.

### Survival Analysis
* **survival** - For core survival modeling (Kaplan-Meier estimates).
* **adjustedCurves** - For calculating and plotting confounder-adjusted survival curves.
* **pammtools** - For piece-wise exponential additive mixed models.

### Flowcharts, Figures & Layouts
* **DiagrammeR** / **DiagrammeRsvg** / **magick** - Used together to programmatically generate and render study flowcharts.
* **ggplot2** - For creating publication-quality statistical graphics.
* **cowplot** / **grid** / **gridExtra** / **gridGraphics** - For combining multiple plots and aligning complex figure grids.

## Data Availability
Individual-level data from the register-linkage used in this study cannot be made publicly available under current European and Swedish legislation for data protection (General Data Protection Regulation (GDPR), Swedish law (SFS 2018:218), the Swedish Data Protection Act, the Swedish Ethical Review Act, and the Public Access to Information and Secrecy Act). 

Researchers who wish to replicate the study findings can request approval from the Swedish Ethical Review Authority (registrator@etikprovning.se) to apply for data extraction directly from the respective register holders:
* **The Swedish National Board of Health and Welfare** (registerservice@socialstyrelsen.se)
* **Statistics Sweden** (information@scb.se)

## Author
**Angelo Mezzoiuso, MD**  
PhD Candidate  
LinkedIn: https://www.linkedin.com/in/angelo-mezzoiuso/
