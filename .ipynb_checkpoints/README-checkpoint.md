Overview
--------

The code in this replication package constructs the analysis file for *Consumer Credit Usage in Canada during the Coronavirus Pandemic* by Ho, Morin, Paarsch and Huynh at the Canadian Journal of Economics.

Latest version of the code is available as GitHub repository [Credit_COVID19_Canada](https://github.com/LeeMorinUCF/Credit_COVID19_Canada).

The workflow proceeds in two stages: 
[Data Processing](#Data-Processing) outlines the operations that produce the data set for statistical analysis.
[Statistical Analysis](#Statistical-Analysis) contains the programs used for statistical analysis on the processed data. 
[Generating Tables and Figures](#Generating-Tables-and-Figures)

### Table of Content
- [Data Availability and Provenance](#Data-Availability-and-Provenance)
- [Computational Requirements](#Computational-Requirements)
- TBA
- [List of Tables and Programs](#List-of-Tables-and-Programs)
- [References](#References)

Needed to be clean up
- [Code for Data Processing](#Code-for-Data-Processing)
- [Code for Statistical Analysis](#Code-for-Statistical-Analysis)
- [Code for Tables](#Code-for-Tables)
- [Code for Figures](#Code-for-Figures)

---

Data Availability and Provenance
----------------------------

### TransUnion&reg; data
The primary data source is annonymized consumer credit data from TransUnion&reg;. 
Data are provided to the Bank of Canada on a monthly basis. Under the contractual agreement with TransUnion&reg;, the data are not publicly available. The Bank of Canada does, however, have a process for external researchers to work with these data. The Bank of Canada's [Financial System Research Center](https://www.bankofcanada.ca/research/financial-system-research-centre/) is a hub for research on household finance. Interested parties, who are Canadian citizens or permanent residents, can contact the Managing Director of Economic and Financial Research ([Jim MacGee](https://www.bankofcanada.ca/profile/james-macgee/)) or Senior Rsearch Officer ([Jason Allen](https://www.bankofcanada.ca/profile/jason-allen/)).
Interested parties are asked to submit a project proposal; the proposal is evaluated by senior officer at the Bank of Canada for feasibility; external researchers do not typically have direct access to the data and must work with a Bank of Canada staff member. An exception is if an external collaborator applies for and is granted temporary employee status -- in this case, the external researcher can access the data so long as they have a Bank of Canada affiliation. All research is vetted by Bank of Canada senior officer prior to publication.

### Regulatory filings by the Bank of Canada (formerly E2)

The Bank of Canada’s historical credit aggregates is available in the [Banking and Financial Statistics table](https://www.bankofcanada.ca/rates/banking-and-financial-statistics/). Since October 2020, Statistics Canada produce monthly credit aggregates that aligns with the concepts of Statistics Canada’s National Balance Sheet Accounts program. These new credit aggregates are available on Statistics Canada’s tables: [Consumer credit, outstanding balances of selected holders](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=1010011701) (CANSIM table 10-10-0117-01). For details, see the Bank of Canada's announcement on [*Bank of Canada and Statistics Canada to move to a single set of credit statistics*](https://www.bankofcanada.ca/2019/10/bank-canada-statistics-canada-move-single-set-credit-statistics/).

Datafile: `CAINC30__ALL_AREAS_1969_2018.csv`

### Nilson Report

> Data on National Income and Product Accounts (NIPA) were downloaded from the U.S. Bureau of Economic Analysis (BEA, 2016). We use Table 30. Data can be downloaded from https://apps.bea.gov/regional/downloadzip.cfm, under "Personal Income (State and Local)", select CAINC30: Economic Profile by County, then download. Data can also be directly downloaded using  https://apps.bea.gov/regional/zip/CAINC30.zip. A copy of the data is provided as part of this archive. The data are in the public domain.

Datafile: `CAINC30__ALL_AREAS_1969_2018.csv`

### 2016 Canadian Census

> Data on National Income and Product Accounts (NIPA) were downloaded from the U.S. Bureau of Economic Analysis (BEA, 2016). We use Table 30. Data can be downloaded from https://apps.bea.gov/regional/downloadzip.cfm, under "Personal Income (State and Local)", select CAINC30: Economic Profile by County, then download. Data can also be directly downloaded using  https://apps.bea.gov/regional/zip/CAINC30.zip. A copy of the data is provided as part of this archive. The data are in the public domain.

Datafile: `CAINC30__ALL_AREAS_1969_2018.csv`

### Summary

Some data **cannot be made** publicly available. The following is a list of dataset used:

| Data file | Source | Notes    | Provided |
|:----------|:-------|:---------|:---------|
| `data/lbd.dta` | TransUnion&reg; | [Licensed](#TransUnion&reg;-data) | Only source program |
| `data/lbd.dta` | TransUnion&reg; | [Licensed](#TransUnion&reg;-data) | Only source program |
| `data/terra.dta` | Bank of Canada | [Regulatory filings on credit aggregates](#Regulatory-filings-by-the-Bank-of-Canada-(formerly-E2)) | Yes |
| `data/terra.dta` | Nilson | [Nilson Report](#Nilson-Report) | Yes |
| `data/reg.dta`| Statistics Canada | [2016 Canadian Census](#2016-Canadian-Census) | Yes |

---

Computational Requirements
---------------------------

### Software Requirements

- Python 3.6.4
  - `pyspark` 0.24.2
- R 3.4.3
  - `tidyr` (0.8.3)
  - `rdrobust` (0.99.4)

Portions of the code use bash scripting, which may require Linux.

### Memory and Runtime Requirements

The ```csv``` files in the Data folder are generated on the Bank of Canada's EDITH 2.0 HPC cluster. Processing the data set requires 36 processor cores and 1 TB memory. Approximate time needed to reproduce the analyses is about 24 hours.

---

Description of programs/code
----------------------------

> INSTRUCTIONS: Give a high-level overview of the program files and their purpose. Remove redundant/ obsolete files from the Replication archive.

- Programs in `programs/01_dataprep` will extract and reformat all datasets referenced above. The file `programs/01_dataprep/master.do` will run them all.
- Programs in `programs/02_analysis` generate all tables and figures in the main body of the article. The program `programs/02_analysis/master.do` will run them all. Each program called from `master.do` identifies the table or figure it creates (e.g., `05_table5.do`).  Output files are called appropriate names (`table5.tex`, `figure12.png`) and should be easy to correlate with the manuscript.
- Programs in `programs/03_appendix` will generate all tables and figures  in the online appendix. The program `programs/03_appendix/master-appendix.do` will run them all. 
- Ado files have been stored in `programs/ado` and the `master.do` files set the ADO directories appropriately. 
- The program `programs/00_setup.do` will populate the `programs/ado` directory with updated ado packages, but for purposes of exact reproduction, this is not needed. The file `programs/00_setup.log` identifies the versions as they were last updated.
- The program `programs/config.do` contains parameters used by all programs, including a random seed. Note that the random seed is set once for each of the two sequences (in `02_analysis` and `03_appendix`). If running in any order other than the one outlined below, your results may differ.

### (Optional, but recommended) License for Code

The code is licensed under a MIT/BSD/GPL/Creative Commons license. See [LICENSE.txt](LICENSE.txt) for details.

Instructions to Replicators
---------------------------

> INSTRUCTIONS: The first two sections ensure that the data and software necessary to conduct the replication have been collected. This section then describes a human-readable instruction to conduct the replication. This may be simple, or may involve many complicated steps. It should be a simple list, no excess prose. Strict linear sequence. If more than 4-5 manual steps, please wrap a master program/Makefile around them, in logical sequences. Examples follow.

- Edit `programs/config.do` to adjust the default path
- Run `programs/00_setup.do` once on a new system to set up the working environment. 
- Download the data files referenced above. Each should be stored in the prepared subdirectories of `data/`, in the format that you download them in. Do not unzip. Scripts are provided in each directory to download the public-use files. Confidential data files requested as part of your FSRDC project will appear in the `/data` folder. No further action is needed on the replicator's part.
- Run `programs/01_master.do` to run all steps in sequence.

### Details

- `programs/00_setup.do`: will create all output directories, install needed ado packages. 
   - If wishing to update the ado packages used by this archive, change the parameter `update_ado` to `yes`. However, this is not needed to successfully reproduce the manuscript tables. 
- `programs/01_dataprep`:  
   - These programs were last run at various times in 2018. 
   - Order does not matter, all programs can be run in parallel, if needed. 
   - A `programs/01_dataprep/master.do` will run them all in sequence, which should take about 2 hours.
- `programs/02_analysis/master.do`.
   - If running programs individually, note that ORDER IS IMPORTANT. 
   - The programs were last run top to bottom on July 4, 2019.
- `programs/03_appendix/master-appendix.do`. The programs were last run top to bottom on July 4, 2019.
- Figure 1: The figure can be reproduced using the data provided in the folder “2_data/data_map”, and ArcGIS Desktop (Version 10.7.1) by following these (manual) instructions:
  - Create a new map document in ArcGIS ArcMap, browse to the folder
“2_data/data_map” in the “Catalog”, with files  "provinceborders.shp", "lakes.shp", and "cities.shp". 
  - Drop the files listed above onto the new map, creating three separate layers. Order them with "lakes" in the top layer and "cities" in the bottom layer.
  - Right-click on the cities file, in properties choose the variable "health"... (more details)

---

List of Tables and Programs
---------------------------

#### Table 1: Divergence from Sample Histograms

This Table contains information from two different modeling
exercises: one for credit-cards and one for HELOCs. 

For credit cards, 
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_Cards_estim.R```. 
Lines 118 to 199 of ```COVID_CJE_Cards_estim.R``` 
generate a file named ```CC_KLD_vs_sample_01.tex```. 

For HELOCs, 
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_HELOCs_estim.R```.
Lines 118 to 199 of ```COVID_CJE_HELOCs_estim.R``` 
generate a file named ```HE_KLD_vs_sample_01.tex```. 

The numbers from these two tables are combined into the file ```Table_1.tex```.

#### Table 2: Divergence from l-Step-Ahead Forecasts

This Table also contains information from two different modeling
exercises: one for credit-cards and one for HELOCs. 

For credit cards, 
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_Cards_estim.R```. 
Lines 363 to 433 of ```COVID_CJE_Cards_estim.R``` 
generate a file named ```CC_KLD_kstep_fixed_vs_monthly_02.tex```. 
The columns under the 

For HELOCs, 
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_HELOCs_estim.R```.
Lines 363 to 433 of ```COVID_CJE_HELOCs_estim.R``` 
generate a file named ```HE_KLD_kstep_fixed_vs_monthly_02.tex```. 

The numbers from these two tables 
corresponding to the model with monthly transition matrices 
are combined into the file ```Table_2.tex```.

#### Table 3: Divergence from l-Step-Ahead Forecasts in Alberta, 2015

The creation of this Table mirrors that of Table 2 on the 
Canadian population during the pandemic, 
except that it is run on a dataset restricted to consumers 
in the province of Alberta during the oil price shock in 2015. 
As with Table 2, it also contains information from two different modeling
exercises: one for credit-cards and one for HELOCs. 


For credit cards, 
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_AB_Cards_estim.R```. 
Lines 366 to 436 of ```COVID_CJE_AB_Cards_estim.R``` 
generate a file named ```AB_CC_KLD_kstep_fixed_vs_monthly_02.tex```. 

For HELOCs, 
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_AB_HELOCs_estim.R```.
Lines 366 to 436 of ```COVID_CJE_AB_HELOCs_estim.R``` 
generate a file named ```AB_HE_KLD_kstep_fixed_vs_monthly_02.tex```. 

The numbers from these two tables 
corresponding to the model with monthly transition matrices 
are combined into the file ```Table_3.tex```.


#### Table A1: Comparison of Accounts at the Credit Agency with Nation-Wide Totals in *The Nilson Report*

The set of numbers in the three leftmost columns
are taken directly from the *The Nilson Report*, 
Issue 1173, April 2020, 
and are available [here](https://nilsonreport.com/publication_newsletter_archive_issue.php?issue=1173). 

To compare with the contents of the TransUnion database, 
we calculated the same summary statistics 
using the sample drawn from the database. 
The remaining information was obtained from running the script
```TU_vs_Nilson_comp.py```, which produced the summary dataset 
called ```TU_vs_BoC_num_accts.csv```, found in the ```Data``` folder. 

---

# Code for Figures

#### Figure 1: Consumers' Outstanding Balances, 2017-2020


For credit cards, in panel (a),
run script ```CC_HE_time_series_figs.R```. 
Lines 91 to 132 generate a file named ```CC_time_series.eps```
from the data in a file named ```tu_agg_bc.csv```. 

For HELOCs, in panel (b),
run script ```CC_HE_time_series_figs.R```. 
Lines 141 to 182 generate a file named ```HE_time_series.eps```
from the data in a file named ```tu_agg_heloc.csv```. 


#### Figure 2: Histograms of Individuals' Balances

For credit cards, in panel (a),
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_Cards_prelim.R```. 
Lines 53 to 71 of ```COVID_CJE_Cards_prelim.R``` 
generate a file named ```CC_hist_grp_sample.eps```. 

For HELOCs, in panel (b),
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_HELOCs_prelim.R```.
Lines 53 to 71 of ```COVID_CJE_HELOCs_prelim.R``` 
generate a file named ```HE_hist_grp_sample.eps```. 


#### Figure 3: Conditional Histograms of Individuals' Balances

For credit cards, in panel (a),
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_Cards_prelim.R```. 
Lines 94 to 177 of ```COVID_CJE_Cards_prelim.R``` 
generate a file named ```CC_3D_probs_discrete_1.eps```. 

For HELOCs, in panel (b),
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_HELOCs_prelim.R```.
Lines 94 to 177 of ```COVID_CJE_HELOCs_prelim.R``` 
generate a file named ```HE_3D_probs_discrete_1.eps```. 


#### Figure 4: Deviations from Histograms (Credit Cards)

For credit cards, in panel (a),
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_Cards_estim.R```. 
Lines 36 to 114 of ```COVID_CJE_Cards_estim.R``` 
generate a set of files named ```CC_sample_dev_pct_2020_MM.eps```. 


#### Figure 5: Deviations from Histograms (HELOCs)

For HELOCs, in panel (b),
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_HELOCs_estim.R```.
Lines 36 to 114 of ```COVID_CJE_HELOCs_estim.R``` 
generate a set of files named ```HE_sample_dev_pct_2020_MM.eps```. 


#### Figure 6: Deviations from Forecasted Credit-Card Balances

For credit cards, in panel (a),
run script ```COVID_CJE_Cards.R```, 
which then runs script ```COVID_CJE_Cards_estim.R```. 
Lines 473 to 506 of ```COVID_CJE_Cards_estim.R``` 
generate a file named ```CC_obs_vs_for_dev_pct_monthly_2020_MM.eps```. 


#### Figure 7: Deviations from Forecasted HELOC Balances

For HELOCs, in panel (b),
run script ```COVID_CJE_HELOCs.R```, 
which then runs script ```COVID_CJE_HELOCs_estim.R```.
Lines 473 to 506 of ```COVID_CJE_HELOCs_estim.R``` 
generate a file named ```HE_obs_vs_for_dev_pct_monthly_2020_MM.eps```. 


#### Figure 8: Consumers' Outstanding Balances, Alberta, 2012-2016


For credit cards, in panel (a),
run script ```CC_HE_time_series_figs.R```. 
Lines 226 to 267 generate a file named ```AB_CC_time_series.eps```
from the data in a file named ```tu_agg_AB_bc.csv```. 

For HELOCs, in panel (b),
run script ```CC_HE_time_series_figs.R```. 
Lines 276 to 317 generate a file named ```AB_HE_time_series.eps```
from the data in a file named ```tu_agg_AB_heloc.csv```. 


#### Figure 9: Deviations from Forecasted Balances in Alberta, October 2015

For credit cards, in panel (a),
run script ```COVID_CJE_AB_Cards.R```, 
which then runs script ```COVID_CJE_AB_Cards_estim.R```. 
Lines 476 to 509 of ```COVID_CJE_AB_Cards_estim.R``` 
generate a file named ```AB_CC_obs_vs_for_dev_pct_monthly_2015-11.eps```. 


For HELOCs, in panel (b),
run script ```COVID_CJE_AB_HELOCs.R``` , 
which then runs script ```COVID_CJE_AB_HELOCs_estim.R```.
Lines 476 to 509 of ```COVID_CJE_AB_HELOCs_estim.R``` 
generate a file named ```AB_HE_obs_vs_for_dev_pct_monthly_2015-11.eps```. 


#### Figure A1.1: Time Series of Aggregate Credit-Card Balances

The two panels, which are both generated with the same script, 
one showing balances and the other showing percent changes of all the series. 
Two of the series were created using the 
sample from the TransUnion database
with the following script: 
```TU_vs_BoC_comp.py```

The other series is derived from an internal database housed at the 
Bank of Canada and collected from regulatory returns. 
It is available on the 
"Banking and Financial Statistics"
Webpage of the Bank of Canada and is called
*Chartered bank selected assets: Month-end (formerly C1)*. 
We use the row of the table labeled "Credit cards". 

Together, the aggregate time-series data are recorded 
in the file ```TU_vs_BoC_totals.csv```.
The figures 
in the file ```TU_vs_BoC_comparison.eps```
are then generated with the script ```CC_TU_vs_BoC_comp_figs.R```, 
on lines 89 to 140.


#### Figure A1.2: Credit Data Coverage for Adults in Canada, by Province

The numbers in this figure were calculated with the scripts
```TU_vs_StatsCan_comp.py``` and 
```CC_TU_vs_StatsCan_comp_fig.R``` in the Code folder. 
It requires the dataset 
```CC_TU_vs_StatsCan.csv```, 
comprising 
the number of credit-card account holders aged 20 and above and
the figures obtained from Statistics Canada 
in the table called
*Estimates of population (2016 Census and administrative data), by age group 
  and sex for July 1st, Canada, provinces, territories, 
  health regions (2018 boundaries) and peer groups, *
Table: 17-10-0134-01.
The file ```CC_TU_vs_StatsCan_comp.eps``` for 
Figure A1.2 is created by running lines 96 to 113 
of the script ```CC_TU_vs_StatsCan_comp_fig.R```.



The provided code reproduces elected tables and figures in the paper, as explained and justified below.


| Figure/Table # | Program                  | Line Number | Output file                      | Note                            |
|-------------------|--------------------------|-------------|----------------------------------|---------------------------------|
| Table 1     | 02_analysis/table1.do    |             | summarystats.csv                 ||
| Table 2     | 02_analysis/table2and3.do| 15          | table2.csv                       ||
| Table 3     | 02_analysis/table2and3.do| 145         | table3.csv                       ||
| Figure 1    | n.a. (no data)           |             |                                  | Source: Herodus (2011)          |
| Figure 2    | 02_analysis/fig2.do      |             | figure2.png                      ||
| Figure 3    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure 4    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure 5    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure 6    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure 7    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure 8    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensedl data      |
| Figure 9    | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure A1.1 | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |
| Figure A1.2 | 02_analysis/fig3.do      |             | figure-robustness.png            | Requires licensed data      |

## References

> INSTRUCTIONS: As in any scientific manuscript, you should have proper references. For instance, in this sample README, we cited "Ruggles et al, 2019" and "DESE, 2019" in a Data Availability Statement. The reference should thus be listed here, in the style of your journal:

Steven Ruggles, Steven M. Manson, Tracy A. Kugler, David A. Haynes II, David C. Van Riper, and Maryia Bakhtsiyarava. 2018. "IPUMS Terra: Integrated Data on Population and Environment: Version 2 [dataset]." Minneapolis, MN: *Minnesota Population Center, IPUMS*. https://doi.org/10.18128/D090.V2

Department of Elementary and Secondary Education (DESE), 2019. "Student outcomes database [dataset]" *Massachusetts Department of Elementary and Secondary Education (DESE)*. Accessed January 15, 2019.

U.S. Bureau of Economic Analysis (BEA). 2016. “Table 30: "Economic Profile by County, 1969-2016.” (accessed Sept 1, 2017).

Inglehart, R., C. Haerpfer, A. Moreno, C. Welzel, K. Kizilova, J. Diez-Medrano, M. Lagos, P. Norris, E. Ponarin & B. Puranen et al. (eds.). 2014. World Values Survey: Round Six - Country-Pooled Datafile Version: http://www.worldvaluessurvey.org/WVSDocumentationWV6.jsp. Madrid: JD Systems Institute.

---

## Acknowledgements

The views expressed are those of the authors; no responsibility for these views
should be attributed to the Bank of Canada; all errors are the responsibility
of the authors. We thank the HPC team at the Bank of Canada for their excellent assistance with EDITH 2.0 High Performance Cluster.















---

# Code for Data Processing

These programs were performed on the Bank of Canada's EDITH 2.0 HPC cluster to process the TransUnion&reg; dataset. These python scripts are stored in the ```Code/Data_Prep``` folder. They required the use of [pyspark](https://spark.apache.org/docs/latest/api/python/) package running on [Apache Spark 2.3.0](https://spark.apache.org/).

## Credit Card Data
1. Run the sequence of python scripts ```cr_use_bc_Y1Y2.py```, for data covering each two-year period 20Y1-20Y2. Each script produce an output file in parquet format, named ```df_acct_Y1Y2.parquet```, that contains processed account-level data.

2. Run the python script ```tu_sample_bc.py``` to aggregate account-level data to individual-level, for obtaining a 
  and generates the dataset ```tu_sample_bc.csv```. 
  This dataset is sufficient to run the 
  analysis of credit-card accounts on the nation-wide sample. 
  
1. Run the script ```tu_sample_AB_bc.slurm```, 
  which runs the script ```tu_sample_AB_bc.py```
  and generates the dataset ```tu_sample_AB_bc.csv```. 
  This dataset is sufficient to run the 
  analysis of credit-card accounts on the Alberta sample. 

## HELOC data  

1. Run the script ```df_ind_heloc.slurm```, 
  which runs a sequence of Python scripts ```cr_use_heloc_Y1Y2.py```, 
  for data covering each two-year period 20Y1-20Y2.
  It then runs ```cr_use_heloc_combine.py```, which
  generates a temporary parquet file ```df_ind.parquet```. 
  This dataset comprises individual-level data 
  that is sufficient to run the data manipulation for 
  HELOC accounts on the nation-wide sample. 

1. Run the script ```tu_sample_heloc.slurm```, 
  which runs the script ```tu_sample_heloc.py```
  and generates the dataset ```tu_sample_heloc.csv```. 
  This dataset is sufficient to run the 
  analysis of HELOC accounts on the nation-wide sample. 
  
1. Run the script ```tu_sample_AB_heloc.slurm```, 
  which runs the script ```tu_sample_AB_heloc.py```
  and generates the dataset ```tu_sample_AB_heloc.csv```. 
  This dataset is sufficient to run the 
  analysis of HELOC accounts on the Alberta sample. 

1. Run the script ```tu_agg_series_heloc.slurm```, 
  which runs the script ```tu_agg_series.py```
  and generates the dataset ```tu_agg_heloc.csv```. 
  This dataset provides the input for
  panel (b) of Figure 1: Consumers' Outstanding Balances, 2017-2020
  for HELOC accounts on the nation-wide sample. 

1. Run the script ```tu_agg_series_AB_heloc.slurm```, 
  which runs the script ```tu_agg_series_AB.py```
  and generates the dataset ```tu_agg_AB_heloc.csv```. 
  This dataset provides the input for
  panel (b) of Figure 8: Consumers' Outstanding Balances, Alberta, 2012-2016
  for HELOC accounts on the Alberta sample. 

---

## Datasets

The above operations will produce the following datasets in ```csv``` format. 

### Main datasets

The ```Data``` folder must contain four main datasets: 
```tu_sample_bc.csv``` and ```tu_sample_heloc.csv```
for the nation-wide sample, 
as well as
```tu_sample_AB_bc.csv``` and ```tu_sample_AB_heloc.csv```
for the sample restricted to the province of Alberta. 


#### tu_sample_bc.csv

This dataset contains observations of credit card balances 
for consumers in Canada from 2017-2021. 
It contains the following variables:

1. ```tu_consumer_id``` is a 9-digit integer that indicates an individual consumer. 
1. ```Run_Date``` is a date variable of the form ```'YYYY-MM-01'``` 
indicating the month in which the data were reported by the bureau. 
It is the date that represents the last information added to the files, 
so it contains the statement activity recorded in the previous month. 
1. ```prov``` is a string that indicates the province of residence of the consumer.
1. ```homeowner``` is an indicator that the consumer has ever had a mortgage or a HELOC loan. 
1. ```N_bc``` is the number of credit card accounts held by a consumer.
1. ```bc_bal``` is the consumer's credit-card balance in dollars. 

#### tu_sample_he.csv

This dataset contains observations of HELOC balances 
for consumers in Canada from 2017-2021. 
It contains the following variables:

1. ```tu_consumer_id``` is an unique identifier for an individual in the data set. 
1. ```Run_Date``` is a date variable of the form ```'YYYY-MM-01'``` indicating the month in which the data were reported. 
1. ```prov``` is a string that indicates the province of residence of the consumer.
1. ```homeowner``` is an indicator that the consumer has ever had a mortgage or a HELOC loan. 
1. ```N_he``` is the number of HELOC accounts held by a consumer.
1. ```he_bal``` is the consumer's HELOC balance in dollars. 

#### tu_sample_AB_bc.csv and tu_sample_AB_bc.csv

These datasets have the same format as for
```tu_sample_bc.csv``` and ```tu_sample_he.csv```, 
described above. 



### Auxiliary datasets

#### Time series plots: nation-wide sample

The ```Data``` folder also contains two datasets for generating 
aggregate time-series in Figure 1. 
The files ```tu_agg_bc.csv``` and ```tu_agg_heloc.csv``` contain 
time series of aggregate statistics throughout the sample. 

These files both contain the following variables.
1. ```Run_Date``` is a date variable of the form ```'YYYY-MM-01'```, 
indicating the month in which the data were reported by the bureau. 
1. ```bal_avg``` is the average balance held by consumers during the month. 
1. ```bal_sd``` is the standard deviation of balances held by consumers during the month. 
1. ```bal_p25``` is the lower quartile of balances held by consumers during the month. 
1. ```bal_p50``` is the median balance held by consumers during the month. 
1. ```bal_p75``` is the upper quartile of balances held by consumers during the month. 


#### Time series plots: Alberta sample

The ```Data``` folder also contains another pair of datasets 
for generating 
aggregate time-series in Figure 8. 
The files ```tu_agg_AB_bc.csv``` and ```tu_agg_AB_heloc.csv``` 
contain time series of aggregate statistics throughout the sample, 
in an identical format, except that these were
restricted to the province of Alberta. 


#### Validation of aggregate credit-card balances

A dataset of time series of aggregate outstanding credit-card balances
is required to generate Figure A1.1. 
These data are stored in a file ```TU_vs_BoC_totals.csv```, 
which includes the following columns.

1. ```Date``` in ```DD/MM/YYYY``` format, representing the last day of each month. 
1. ```MCP``` is a series drawn from the Webpage of the Bank of Canada, entitled
*Chartered bank selected assets: Month-end (formerly C1), Credit cards*, 
which records assets in the personal loan category of non-mortgage loans.
1. ```tot_bal_all``` is the aggregate credit-card balance, in billions of Canadian dollars,
  across all institutions represented in the TransUnion database. 
1. ```tot_bal_bank``` is the aggregate credit-card balance, in billions of Canadian dollars,
  across the chartered banks. 


#### Validation of credit-card data coverage by province

A dataset of aggregate counts of the number of cardholders by province 
was compared to the population in each province in Figure A1.2. 
This information was collected in the dataset ```CC_TU_vs_StatsCan.csv```, 
with the following columns. 

1. ```region``` is the two-letter abbreviation of each province in Canada.
1. ```N_geq20_BC``` is the number of consumers aged 20 and above 
holding accounts during the month of January 2016. 
1. ```geq20``` is the population of each province in the age categories 20 and above, 
which was obtained from Statstics Canada Table: 17-10-0134-01, described below.



#### Other data

Other data were obtained to produce tables and figures of 
aggregate information about the credit-card and HELOC markets. 
These include:
- The Nilson Report, April 2020, Issue 1173, HSN Consultants, Inc.
- Chartered bank selected assets: Month-end (formerly C1), Credit cards, 
  Bank of Canada, accessed June 2020. 
- Estimates of population (2016 Census and administrative data), by age group 
  and sex for July 1st, Canada, provinces, territories, 
    health regions (2018 boundaries) and peer groups, Table: 17-10-0134-01, 
    Statistics Canada, accessed June 2020. 



These data sources are used in the statistical analysis that follows. 

### Data Validation

1. Run the script ```tu_agg_series.py``` to generate the dataset ```tu_agg_bc.csv```. This dataset provides the input for panel (a) of Figure 1: Consumers' Outstanding Balances, 2017-2020 for credit-card accounts on the nation-wide sample. 

1. Run the script ```tu_agg_series_AB.py``` to generate the dataset ```tu_agg_AB_bc.csv```. This dataset provides the input for   panel (a) of Figure 8: Consumers' Outstanding Balances, Alberta, 2012-2016 for credit-card accounts on the Alberta sample. 
  
1. Run the script ```TU_vs_Nilson_comp.py```to generate the dataset ```TU_vs_Nilson_num_accts.csv```. This dataset provides the input for Table A1: Comparison of Accounts at the Credit Agency with Nation-Wide Totals in The Nilson Report. 
  
1. Run the script ```TU_vs_BoC_comp.py``` to generate the dataset ```TU_vs_BoC_totals.csv```. This dataset provides the input for Figure A1.1: Time Series of Aggregate Credit-Card Balances. 
  
1. Run the script ```TU_vs_StatsCan_comp.py``` to generate the dataset ```CC_TU_vs_StatsCan.csv```. This dataset provides the input for Figure A1.2: Credit Data Coverage for Adults in Canada, by Province. 
  

---

## Statistical Analysis

These procedures were performed on a microcomputer
to generate the tables and figures in the paper.
These scripts are stored in the ```Code/Stats``` folder. 

### All Files in One Script:

1. Place all datasets in the ```Data``` folder, 
including the main datasets 
```tu_sample_bc.csv```, ```tu_sample_heloc.csv```, 
```tu_sample_AB_bc.csv```, and ```tu_sample_AB_heloc.csv```, 
along with the auxiliary datasets for time-series plots
```tu_agg_bc.csv```, ```tu_agg_heloc.csv```, 
```tu_agg_AB_bc.csv```, and ```tu_agg_AB_heloc.csv```, 
and for figures in the appendix
```TU_vs_BoC_num_accts.csv``` and ```CC_TU_vs_StatsCan.csv```.
 
1. Run ```COVID_CJE.sh``` in a terminal window from the ```Credit_COVID19_Canada``` folder. 


This shell script calls the main ```R``` programs 
```COVID_CJE_Cards.R```, ```COVID_CJE_HELOCs.R``` 
```COVID_CJE_AB_Cards.R```, ```COVID_CJE_AB_HELOCs.R``` 
as well as the auxiliary ```R``` scripts 
```CC_HE_time_series_figs.R```, 
```CC_BoC_vs_TU_comp_figs.R```, and 
```CC_TU_vs_StatsCan_comp_fig.R```,
all found in the ```Code/Stats``` folder, 
which analyze the datasets stored in the ```Data``` folder. 
These scripts create the tables and figures for the entire manuscript,
by writing ```tex``` files to the ```Tables``` folder and
```eps``` files to the ```Figures``` folder. 


### Generating Sets of Files

#### Nation-Wide Sample of Credit-Card Accounts

1. Place the dataset 
```tu_sample_bc.csv```
in the ```Data``` folder. 
1. Run ```Rscript COVID_CJE_Cards.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 


1. Obtain the ```tex``` files 
```CC_KLD_vs_sample_01.tex``` and 
```CC_KLD_kstep_fixed_vs_monthly_02.tex```
with numbers for columns 2 and 3 of Tables 1 and 2 
from the ```Tables``` folder. 

1. Obtain the images
for panels (a) of Figures 2 and 3 in the ```eps``` files
```CC_hist_grp.eps``` and 
```CC_3D_probs_discrete_1.eps```
from the ```Figures``` folder.

1. Obtain the images
for Figures 4 and 6 in the ```eps``` files
```CC_sample_dev_pct_2020_MM.eps``` and 
```CC_obs_vs_for_dev_pct_monthly_2020-MM.eps```
from the ```Figures``` folder, 
where ```MM``` represents the two-digit month of the 
```Run_date``` after the close of the corresponding statement month. 


#### Nation-Wide Sample of HELOC Accounts

1. Place the dataset
```tu_sample_heloc.csv```
in the ```Data``` folder. 
1. Run ```Rscript COVID_CJE_HELOCs.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 


1. Obtain the ```tex``` files 
```HE_KLD_vs_sample_01.tex``` and 
```HE_KLD_kstep_fixed_vs_monthly_02.tex```
with numbers for columns 4 and 5 of Tables 1 and 2 
from the ```Tables``` folder. 

1. Obtain the images
for panels (b) of Figures 2 and 3 in the ```eps``` files
```HE_hist_grp.eps``` and 
```HE_3D_probs_discrete_1.eps```
from the ```Figures``` folder.

1. Obtain the images
for Figures 5 and 7 in the ```eps``` files
```HE_sample_dev_pct_2020_MM.eps``` and 
```HE_obs_vs_for_dev_pct_monthly_2020-MM.eps```
from the ```Figures``` folder, 
where ```MM``` represents the two-digit month of the 
```Run_date``` after the close of the corresponding statement month. 



#### Alberta Sample of Credit-Card Accounts

1. Place the dataset 
```tu_sample_AB_bc.csv```
in the ```Data``` folder. 
1. Run ```Rscript COVID_CJE_AB_Cards.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 
1. Obtain the ```tex``` file ```AB_CC_KLD_kstep_fixed_vs_monthly_02.tex``` 
with numbers for columns 2 and 3 for Table 3 
in the ```Tables``` folder
and panel (a) of Figure 9 in the file
```AB_CC_obs_vs_for_dev_pct_monthly_2015-11.eps```
in the ```Figures``` folder.


#### Alberta Sample of HELOC Accounts

1. Place the dataset 
```tu_sample_AB_heloc.csv```
in the ```Data``` folder. 
1. Run ```Rscript COVID_CJE_AB_HELOCs.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 
1. Obtain the ```tex``` file ```AB_HE_KLD_kstep_fixed_vs_monthly_02.tex```
with numbers for columns 4 and 5 for Table 3 
in the ```Tables``` folder
and panel (b) of Figure 9 in the file
```AB_HE_obs_vs_for_dev_pct_monthly_2015-11.eps```
in the ```Figures``` folder.


#### Auxilliary Tables and Figures

Instructions for generating the remaining tables and figures are outlined in the next section "Generating Tables and Figures Separately".