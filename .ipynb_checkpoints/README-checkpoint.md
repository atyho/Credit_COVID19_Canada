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
- [Description of Programs/code](#Description-of-Programs/code)
    - [Code for Data Processing](#Code-for-Data-Processing)
    - [Code for Statistical Analysis](#Code-for-Statistical-Analysis)
- [List of Tables and Programs](#List-of-Tables-and-Programs)
- [References](#References)

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

The set of numbers in the three leftmost columns
are taken directly from the *The Nilson Report*, 
Issue 1173, April 2020, 
and are available [here](https://nilsonreport.com/publication_newsletter_archive_issue.php?issue=1173). 

Datafile: `CAINC30__ALL_AREAS_1969_2018.csv`

### 2016 Canadian Census

> Data on National Income and Product Accounts (NIPA) were downloaded from the U.S. Bureau of Economic Analysis (BEA, 2016). We use Table 30. Data can be downloaded from https://apps.bea.gov/regional/downloadzip.cfm, under "Personal Income (State and Local)", select CAINC30: Economic Profile by County, then download. Data can also be directly downloaded using  https://apps.bea.gov/regional/zip/CAINC30.zip. A copy of the data is provided as part of this archive. The data are in the public domain.

the number of credit-card account holders aged 20 and above and
the figures obtained from Statistics Canada 
in the table called
*Estimates of population (2016 Census and administrative data), by age group 
  and sex for July 1st, Canada, provinces, territories, 
  health regions (2018 boundaries) and peer groups, *
Table: 17-10-0134-01.

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

- Apache Spark 2.3.0
- Python 3.6.4
  - `pyspark` 0.24.2
- R 3.4.3
  - `tidyr` (0.8.3)
  - `rdrobust` (0.99.4)

Portions of the code use bash scripting, which may require Linux.

### Memory and Runtime Requirements

The ```csv``` files in the Data folder are generated on the Bank of Canada's EDITH 2.0 HPC cluster. Processing the data set requires 36 processor cores and 1 TB memory. Approximate time needed to reproduce the analyses is about 24 hours.

---

Description of Programs/code
---------------------------

- Programs in `programs/01_dataprep` will extract and reformat all datasets referenced above. The file `programs/01_dataprep/master.do` will run them all.
- Programs in `programs/03_appendix` will generate all tables and figures  in the online appendix. The program `programs/03_appendix/master-appendix.do` will run them all. 

### Instructions to Replicators

- Edit `programs/config.do` to adjust the default path
- Run `programs/00_setup.do` once on a new system to set up the working environment. 
- Download the data files referenced above. Each should be stored in the prepared subdirectories of `data/`, in the format that you download them in. Do not unzip. Scripts are provided in each directory to download the public-use files. Confidential data files requested as part of your FSRDC project will appear in the `/data` folder. No further action is needed on the replicator's part.

### Code for Data Processing

These programs were performed on the Bank of Canada's EDITH 2.0 HPC cluster to process the TransUnion&reg; dataset. These python scripts are stored in the ```Code/Data_Prep``` folder. They required the use of [pyspark](https://spark.apache.org/docs/latest/api/python/) package running on [Apache Spark 2.3.0](https://spark.apache.org/).

#### Credit Card data

1. Run the sequence of python scripts ```cr_use_bc_Y1Y2.py```, for data covering each two-year period 20Y1-20Y2. Each script produce an output file in parquet format, named ```df_acct_Y1Y2.parquet```, that contains processed account-level data

2. Run python script ```tu_sample_bc.py``` to aggregate account-level data to individual-level, for obtaining a  and generates the dataset ```tu_sample_bc.csv```. 
  This dataset is sufficient to run the 
  analysis of credit-card accounts on the nation-wide sample. 
  
3. Run python script ```tu_sample_AB_bc.py``` to generate the 1% sample of individuals in the Alberta sample. 

These datasets contain the following variables:
- ```tu_consumer_id``` is a 9-digit integer that indicates an individual consumer 
- ```Run_Date``` is a date variable of the form ```'YYYY-MM-01'``` indicating the month in which the data were reported 
- ```prov``` is a string that indicates the province of residence of the consumer
- ```homeowner``` is an indicator that the consumer has ever had a mortgage or a HELOC loan 
- ```N_bc``` is the number of credit card accounts held by a consumer
- ```bc_bal``` is the consumer's credit-card balance in dollars

#### HELOC data  

1. Run the  sequence of python scripts ```cr_use_heloc_Y1Y2.py```, for data covering each two-year period 20Y1-20Y2.   It then runs ```cr_use_heloc_combine.py```, which
  generates a temporary parquet file ```df_ind.parquet```. 

2. Run the script ```tu_sample_heloc.slurm```, 
  which runs the script ```tu_sample_heloc.py```
  and generates the dataset ```tu_sample_heloc.csv```. 
  This dataset is sufficient to run the 
  analysis of HELOC accounts on the nation-wide sample. 
  
3. Run the script ```tu_sample_AB_heloc.slurm```, 
  which runs the script ```tu_sample_AB_heloc.py```
  and generates the dataset ```tu_sample_AB_heloc.csv```. 
  This dataset is sufficient to run the 
  analysis of HELOC accounts on the Alberta sample. 

These datasets contain the following variables:
- ```tu_consumer_id``` is an unique identifier for an individual in the data set
- ```Run_Date``` is a date variable of the form ```'YYYY-MM-01'``` indicating the month in which the data were reported
- ```prov``` is a string that indicates the province of residence of the consumer
- ```homeowner``` is an indicator that the consumer has ever had a mortgage or a HELOC loan 
- ```N_he``` is the number of HELOC accounts held by a consumer
- ```he_bal``` is the consumer's HELOC balance in dollars

### Code for Statistical Analysis

These procedures were performed on a desktop machine to generate the tables and figures in the paper. These scripts are stored in the ```Code/Stats``` folder. 

#### All Files in One Script:

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

#### Nation-wide Sample of Credit-Card Accounts

1. Run ```Rscript COVID_CJE_Cards.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 

1. Obtain the ```tex``` files ```CC_KLD_vs_sample_01.tex``` and ```CC_KLD_kstep_fixed_vs_monthly_02.tex``` with numbers for columns 2 and 3 of Tables 1 and 2 from the ```Tables``` folder. 

1. Obtain the images for panels (a) of Figures 2 and 3 in the ```eps``` files ```CC_hist_grp.eps``` and ```CC_3D_probs_discrete_1.eps``` from the ```Figures``` folder.

1. Obtain the images for Figures 4 and 6 in the ```eps``` files
```CC_sample_dev_pct_2020_MM.eps``` and 
```CC_obs_vs_for_dev_pct_monthly_2020-MM.eps```
from the ```Figures``` folder, 
where ```MM``` represents the two-digit month of the 
```Run_date``` after the close of the corresponding statement month. 

##### Nation-Wide Sample of HELOC Accounts

1. Run ```Rscript COVID_CJE_HELOCs.R``` in a terminal window from the ```Credit_COVID19_Canada``` folder. 

1. Obtain the ```tex``` files ```HE_KLD_vs_sample_01.tex``` and ```HE_KLD_kstep_fixed_vs_monthly_02.tex``` with numbers for columns 4 and 5 of Tables 1 and 2 
from the ```Tables``` folder. 

1. Obtain the images for panels (b) of Figures 2 and 3 in the ```eps``` files ```HE_hist_grp.eps``` and ```HE_3D_probs_discrete_1.eps``` from the ```Figures``` folder.

1. Obtain the images for Figures 5 and 7 in the ```eps``` files ```HE_sample_dev_pct_2020_MM.eps``` and ```HE_obs_vs_for_dev_pct_monthly_2020-MM.eps``` from the ```Figures``` folder, where ```MM``` represents the two-digit month of the ```Run_date``` after the close of the corresponding statement month. 

##### Alberta Sample of Credit-Card Accounts

1. Run ```Rscript COVID_CJE_AB_Cards.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 
2. Obtain the ```tex``` file ```AB_CC_KLD_kstep_fixed_vs_monthly_02.tex``` 
with numbers for columns 2 and 3 for Table 3 
in the ```Tables``` folder
and panel (a) of Figure 9 in the file
```AB_CC_obs_vs_for_dev_pct_monthly_2015-11.eps```
in the ```Figures``` folder.

##### Alberta Sample of HELOC Accounts

1. Run ```Rscript COVID_CJE_AB_HELOCs.R``` 
in a terminal window from the ```Credit_COVID19_Canada``` folder. 
1. Obtain the ```tex``` file ```AB_HE_KLD_kstep_fixed_vs_monthly_02.tex```
with numbers for columns 4 and 5 for Table 3 
in the ```Tables``` folder
and panel (b) of Figure 9 in the file
```AB_HE_obs_vs_for_dev_pct_monthly_2015-11.eps```
in the ```Figures``` folder.

#### Auxiliary datasets

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

### Data Validation

1. Run the script ```tu_agg_series.py``` to generate the dataset ```tu_agg_bc.csv```. This dataset provides the input for panel (a) of Figure 1: Consumers' Outstanding Balances, 2017-2020 for credit-card accounts on the nation-wide sample. 

1. Run the script ```tu_agg_series_AB.py``` to generate the dataset ```tu_agg_AB_bc.csv```. This dataset provides the input for   panel (a) of Figure 8: Consumers' Outstanding Balances, Alberta, 2012-2016 for credit-card accounts on the Alberta sample. 
  
1. Run the script ```TU_vs_Nilson_comp.py```to generate the dataset ```TU_vs_Nilson_num_accts.csv```. This dataset provides the input for Table A1: Comparison of Accounts at the Credit Agency with Nation-Wide Totals in The Nilson Report. 
  
1. Run the script ```TU_vs_BoC_comp.py``` to generate the dataset ```TU_vs_BoC_totals.csv```. This dataset provides the input for Figure A1.1: Time Series of Aggregate Credit-Card Balances. 
  
1. Run the script ```TU_vs_StatsCan_comp.py``` to generate the dataset ```CC_TU_vs_StatsCan.csv```. This dataset provides the input for Figure A1.2: Credit Data Coverage for Adults in Canada, by Province. 
  
### License for Code

The code is licensed under a MIT/BSD/GPL/Creative Commons license. See [LICENSE.txt](LICENSE.txt) for details.



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

The provided code reproduces elected tables and figures in the paper, as explained and justified below.

| Figure/Table # | Program | Line Number | Output file | Note |
|:---|:---|:---|:---|:---|
| Table 1     | COVID_CJE_Cards_estim.R <br> COVID_CJE_HELOCs_estim.R | 118-199 | CC_KLD_vs_sample_01.tex<br>HE_KLD_vs_sample_01.tex | Combined into Table 1 |
| Table 2     | COVID_CJE_Cards_estim.R <br> COVID_CJE_HELOCs_estim.R | 363-433 | CC_KLD_kstep_fixed_vs_monthly_02.tex <br> HE_KLD_kstep_fixed_vs_monthly_02.tex | Combined into Table 2 |
| Table 3     | COVID_CJE_AB_Cards_estim.R <br> COVID_CJE_AB_HELOCs_estim.R | 366-436 | AB_CC_KLD_kstep_fixed_vs_monthly_02.tex <br> AB_HE_KLD_kstep_fixed_vs_monthly_02.tex | Combined into Table 3 |
| Table A1    | TU_vs_Nilson_comp.py |  | TU_vs_BoC_num_accts.csv | |
| Figure 1    | CC_HE_time_series_figs.R <br> CC_HE_time_series_figs.R | 91-132 <br> 141-182 | CC_time_series.eps <br> HE_time_series.eps | Require provided data files ```tu_agg_bc.csv``` and ```tu_agg_heloc.csv``` |
| Figure 2    | COVID_CJE_Cards_prelim.R <br> COVID_CJE_HELOCs_prelim.R | 53-71 | CC_hist_grp_sample.eps <br> HE_hist_grp_sample.eps | |
| Figure 3    | COVID_CJE_Cards_prelim.R <br> COVID_CJE_HELOCs_prelim.R | 94-177 | CC_3D_probs_discrete_1.eps <br> HE_3D_probs_discrete_1.eps | Requires licensed data |
| Figure 4    | COVID_CJE_Cards_estim.R | 36-114 | CC_sample_dev_pct_2020_MM.eps | Requires licensed data |
| Figure 5    | COVID_CJE_HELOCs_estim.R | 36-114 | HE_sample_dev_pct_2020_MM.eps | Requires licensed data |
| Figure 6    | COVID_CJE_Cards_estim.R | 473-506 | CC_obs_vs_for_dev_pct_monthly_2020_MM.eps | Requires licensed data |
| Figure 7    | COVID_CJE_HELOCs_estim.R | 473-506 | HE_obs_vs_for_dev_pct_monthly_2020_MM.eps | Requires licensed data |
| Figure 8    | CC_Cards_time_series_figs.R <br> CC_HE_time_series_figs.R | | AB_CC_time_series.eps <br> AB_HE_time_series.eps | tu_agg_AB_bc.csv and tu_agg_AB_heloc.csv |
| Figure 9    | COVID_CJE_AB_Cards_estim.R <br> COVID_CJE_AB_HELOCs_estim.R | 476-509 | AB_CC_obs_vs_for_dev_pct_monthly_2015-11.eps <br> AB_HE_obs_vs_for_dev_pct_monthly_2015-11.eps | Requires licensed data |
| Figure A1.1 | CC_TU_vs_BoC_comp_figs.R | | TU_vs_BoC_comparison.eps | Requires licensed data |
| Figure A1.2 | CC_TU_vs_StatsCan_comp_fig.R | | CC_TU_vs_StatsCan_comp.eps | Requires licensed data |

---

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