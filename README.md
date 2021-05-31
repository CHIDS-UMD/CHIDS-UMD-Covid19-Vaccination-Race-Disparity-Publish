# Socioeconomic Privilege and Political Ideology are Associated with Racial Disparity in COVID-19 Vaccination: Methods and Materials

This public repository contains the materials for reproducing the results described in Agarwal et al. (2021) _Socioeconomic Privilege and Political Ideology Are Associated with Racial Disparity in COVID-19 Vaccination_ and additional supplementary analyses. 


## Table of Contents

- [Content Description](#materials)

- [Data Sources for COVID-19 Racial Vaccination by States](#covid_race_data_by_state)

    * [COVID-19 Vaccination Disparity Map](#covid_map)

- [Definition of Vaccination Rate Disparity](#vax_rate_disparity)

- [Missingness](#missing)

- [Summary Statistics for Final Data](#summary_statistics)

    * [Correlation Matrix Heatmap for Final Data](#correlation_map)

- [Main Regression Result](#main_reg)

    * [Base Model Regression Table](#base_model)

    * [Coefficient Plot for COVID-19 and Flu Vaccination Disparity Analysis](#coef_plot)


- [Robustness Checks](#robustness_checks)

    * [Age Group Controls](#age_control)
    
    * [Different Disparity Measurements](#disparity_measure)
    
    * [Different Dates and Different Vaccination Rate Types](#date_ratetype)
    
    * [Exodus](#exodus_test)
    
    * [Recent Positive Rate per COVID-19 Test](#positivity)


<a name="materials"/>

## Content Description
Materials for reproducibility include:

1. [COVID-19 vaccination rate data](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/tree/main/CountyVaccine) and Python code to reproduce the data collection, including:<br>

    a) The notebook [1.CountyVaccine_Automation](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/blob/main/1.CountyVaccine_Automation.ipynb) includes the code to collect the county-level vaccination information by race from the States whose vaccination data is oragnized in a downlable table. In this notebook, the Python code can automatically scrape the data. The States include： Illinois, Texas, Pennsylvania, Indiana, and Virginia. <br>
    
    b) The notebook [1.CountyVaccine_Tableau](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/blob/main/1.CountyVaccine_Tableau.ipynb) is designed to collect the county-level vaccination information by race from the States whose vaccination information is present in a Tableau Dashboard format. In this notebook, the Python code can also automatically scrape the data. The States include：New York, Wisconsin, Ohio, South Carolina, and Oregon.<br>
    
    c) The notebook [1.CountyVaccine_Manual](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/blob/main/1.CountyVaccine_Manual.ipynb) is developed to collect the county-level racial vaccination information from the States whose vaccination information needs to be collected manually before running the code. These States include: California, Tennessee, North Carolina, West Virginia, Maine, and New Jersey. The instructions on manual collections are documented [here](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/tree/main/CountyVaccine/Documents/Part1).<br> 
    
2. [Data](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/tree/main/DataMerge) and [Python code](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/blob/main/2.DataMerge.ipynb) to merge information from the various sources cited in our Supplementary Information (SI) Appendix.

3. Python code for [cleaning the data](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/blob/main/2.DataClean.ipynb). 

4. [Clean data](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/tree/main/StataReg) and [code](https://github.com/CHIDS-UMD/Covid19-Vaccination-Race-Disparity/blob/main/3.StataCode.ipynb) to reproduce our main regression analyses (reported in main text) and robustness checks (reported in SI Appendix) as well as  additional supplementary analyses reported here. 

Below, we also provide additional summary statistics, exploratory data analysis, and full results for the robustness checks described in the SI appendix. 



<a name="covid_race_data_by_state"/>

## Data Sources for COVID-19 Racial Vaccination by States

| State          | # of Counties | Population (million) | # of Valid Counties | Population in Analysis (million) | Data Source                                                                                                                                                   |
|----------------|---------------|----------------------|---------------------|----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| California     | 54            | 39.45                | 43                  | 39.09                            | https://covid19.ca.gov/vaccines/#California-vaccines-dashboard                                                                                                |
| Illinois       | 100           | 12.46                | 41                  | 11.41                            | https://www.dph.illinois.gov/covid19/vaccinedata?county=Illinois                                                                                              |
| Indiana        | 88            | 6.62                 | 37                  | 5.31                             | https://www.coronavirus.in.gov/vaccine/2680.htm                                                                                                               |
| Maine          | 16            | 1.34                 | 8                   | 1.01                             | https://www.maine.gov/covid19/vaccines/dashboard                                                                                                              |
| New Jersey     | 21            | 8.88                 | 21                  | 8.88                             | https://www.nj.gov/health/cd/topics/covid2019_dashboard.shtml                                                                                                 |
| New York       | 62            | 19.45                | 48                  | 18.79                            | https://covid19vaccine.health.ny.gov/covid-19-vaccine-tracker                                                                                                 |
| North Carolina | 43            | 8.56                 | 43                  | 8.56                             | https://covid19.ncdhhs.gov/dashboard/data-behind-dashboards                                                                                                   |
| Ohio           | 88            | 11.69                | 57                  | 10.57                            | https://coronavirus.ohio.gov/wps/portal/gov/covid-19/dashboards/covid-19-vaccine/covid-19-vaccination-dashboard                                               |
| Oregon         | 23            | 4.05                 | 11                  | 3.22                             | https://public.tableau.com/profile/oregon.health.authority.covid.19#!/vizhome/OregonCOVID-19VaccinationTrends/OregonCountyVaccinationTrends                   |
| Pennsylvania   | 60            | 12.68                | 40                  | 11.66                            | https://www.health.pa.gov/topics/disease/coronavirus/Vaccine/Pages/Vaccine.aspx                                                                               |
| South Carolina | 46            | 5.15                 | 46                  | 5.15                             | https://scdhec.gov/covid19/covid-19-vaccination-dashboard                                                                                                     |
| Tennessee      | 90            | 6.77                 | 62                  | 6.11                             | https://www.tn.gov/health/cedep/ncov/data/downloadable-datasets.html                                                                                          |
| Texas          | 236           | 27.66                | 137                 | 26.71                            | https://tabexternal.dshs.texas.gov/t/THD/views/COVID-19VaccineinTexasDashboard/Summary?%3Aorigin=card_share_link&%3Aembed=y&%3AisGuestRedirectFromVizportal=y |
| Virginia       | 132           | 8.53                 | 111                 | 8.21                             | https://www.vdh.virginia.gov/coronavirus/covid-19-vaccine-demographics/                                                                                       |
| West Virginia  | 55            | 1.79                 | 23                  | 1.27                             | https://dhhr.wv.gov/COVID-19/Pages/default.aspx                                                                                                               |
| Wisconsin      | 72            | 5.82                 | 28                  | 4.69                             | https://www.dhs.wisconsin.gov/covid-19/vaccine-data.htm#day                                                                                                   |
| Sum            | 1186          | 180.92               | 756                 | 170.65                           |                                                                                                                                                               |

_Note_. Valid counties are those that were included in our main regression analyses, following the exclusion criteria outlined below in the sample construction figure. 

<a name="covid_map"/>

### COVID-19 Vaccination Disparity Map

![](_img/map.png)


**Figure S1.** Map represents COVID-19 vaccination disparities across 1,186 counties with data by race as of April 19, 2021. Red indicates higher vaccination rates among Whites, and blue indicates higher vaccination rates among Blacks. The vaccination rate in some counties with small numbers of Blacks or Whites exceeded 100%. We exclude those counties in Figure 1. In the regression using data from 756 counties, the range of vaccination disparity is between -52.0% and 66.2%. 


<a name="vax_rate_disparity"/>

## Definition of Vaccination Rate Disparity


<a name="missing"/>

## Missingness


![](_img/missing.png)
**Figure S2.** Patterns of missingness in predictor and outcome variables where white lines indicate values are missing.



<a name="sample_construction"/>

## Sample Construction

The filtering of the data collected for all counties on April 19, 2021. The same method is also applied to the data on March 27 2021, April 07 2021, May 20 2021.

![](_img/Workflow.png)


**Figure S3.** Flowchart depicting sample construction.


<a name="summary_statistics"/>

## Summary Statistics for Final Data

We present descriptive statistics of the variables in our regression analysis in non-standardized units. The table below presents rate and proportion data as percentages for ease of interpretation.  	

| Variable                                 | Description                                                                                            | Source                                                                                                                                                                                                                                            | Data Field             | count | mean   | std    | min     | 0.250  | 0.500  | 0.750  | max     |
|------------------------------------------|--------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|-------|--------|--------|---------|--------|--------|--------|---------|
| CVD                                      | Covid vaccination disparity between White and Black populations in county i.                           | Department of Health in each state                                                                                                                                                                                                                | Vax_DisparityY         | 756   | 12.119 | 10.355 | -18.168 | 4.922  | 10.451 | 17.757 | 53.842  |
| FVD                                      | Flu vaccination disparity between White and Black populations in county i.                             | CMS Mapping Medical Disparity Tool (https://data.cms.gov/mapping-medicare-disparities)                                                                                                                                                            | FluVax_DisparityY      | 756   | 15.185 | 6.445  | -12.000 | 12.000 | 16.000 | 19.000 | 37.000  |
| Median Income                            | Household median income (in thousands) in county i.                                                    | 2019 ACS 5-Year Estimates Subject Tables                                                                                                                                                                                                          | MedianIncome           | 756   | 58.448 | 16.597 | 27.063  | 48.006 | 54.517 | 64.081 | 142.299 |
| Median Income Disparity                  | Household median income disparity (in thousands) between White and Black populations in county i.      | 2019 ACS 5-Year Estimates Subject Tables                                                                                                                                                                                                          | MedianIncome_Disparity | 756   | 21.003 | 16.109 | -67.705 | 13.660 | 21.828 | 29.514 | 112.332 |
| High School Graduation Rate              | Rate of high school or above education attainment in county i.                                         | 2019 ACS 5-Year Estimates Subject Tables                                                                                                                                                                                                          | HighSchool_Rate        | 756   | 86.931 | 5.305  | 61.800  | 83.975 | 87.900 | 90.900 | 96.800  |
| High School Disparity                    | High school or above education attainment disparity between White and Black populations in county i.   | 2019 ACS 5-Year Estimates Subject Tables                                                                                                                                                                                                          | HighSchool_Disparity   | 756   | 6.762  | 7.881  | -30.300 | 2.400  | 6.200  | 10.700 | 42.200  |
| Health Facilities Per Capita             | Number of potential health facilities which provide COVID-19 vaccine per person in county i.           | VaxMap 2.0 (https://www.westhealth.org/resource/vaxmap-potential-covid-19-vaccine-locations/)                                                                                                                                                     | FacNumRate             | 756   | 0.024  | 0.012  | 0.000   | 0.017  | 0.021  | 0.026  | 0.098   |
| COVID-19 Cases Per Capita                | Number of COVID-19 cases per person in county i by April 19th, 2021                                    | The Center for Systems Science and Engineering (CSSE) at Johns Hopkins University (https://github.com/CSSEGISandData/COVID-19)                                                                                                                    | CaseRate               | 756   | 9.469  | 2.657  | 2.235   | 7.720  | 9.527  | 11.116 | 24.159  |
| Home IT Rate                             | Rate of computer ownership and internet in the home in county i.                                       | 2019 ACS 5-Year Estimates Subject Tables                                                                                                                                                                                                          | IT_Rate                | 756   | 82.684 | 7.053  | 55.600  | 78.775 | 83.400 | 87.600 | 97.000  |
| Home IT Disparity                        | Computer ownership and internet in the home disparity between White and Black populations in county i. | 2019 ACS 5-Year Estimates Subject Tables                                                                                                                                                                                                          | IT_Disparity           | 756   | 8.308  | 10.520 | -21.900 | 2.100  | 7.700  | 14.000 | 72.300  |
| Urban                                    | Dummy variable that equals 1 if county i is located in an urban area.                                  | CMS Mapping Medical Disparity Tool (https://data.cms.gov/mapping-medicare-disparities)                                                                                                                                                            | urban                  | 756   | 0.603  | 0.490  | 0.000   | 0.000  | 1.000  | 1.000  | 1.000   |
| Rate of Vehicle Ownership                | Rate of households with vehicles in county i.                                                          | CDC Social Vulnerability Index (https://www.atsdr.cdc.gov/placeandhealth/svi/index.html)                                                                                                                                                          | vehicle                | 756   | 92.914 | 4.962  | 23.000  | 91.800 | 93.700 | 95.200 | 98.600  |
| Political Ideology                       | Rate of people who voted Republican in 2020 presidential election in county i.                         | USA Today (https://www.usatoday.com/in-depth/graphics/2020/11/10/election-maps-2020-america-county-results-more-voters/6226197002/)                                                                                                               | republican_rate        | 756   | 58.749 | 16.065 | 11.249  | 48.080 | 60.414 | 71.550 | 89.324  |
| Segregation Index                        | The degree to which Black and White groups live separately from one another in county i.               | 2021 County Health Rankings (https://www.countyhealthrankings.org/explore-health-rankings/measures-data-sources/county-health-rankings-model/health-factors/social-and-economic-factors/family-social-support/residential-segregation-blackwhite) | Segregation            | 756   | 44.852 | 16.125 | 0.041   | 33.277 | 45.831 | 56.441 | 86.159  |
| Racial Bias                              | Weighted implicit racial bias in county i.                                                             | Data from Riddle and Sinclair (2019; https://osf.io/pu79a/)                                                                                                                                                                                       | racial_weighted_bias   | 756   | 39.993 | 1.879  | 31.156  | 39.199 | 40.177 | 40.888 | 44.453  |
| Vaccine Hesitancy                        | COVID-19 vaccine hesitancy in county i.                                                                | Department of Health and Human Services, Office of the Assistant Secretary for Planning and Evaluation (https://aspe.hhs.gov/pdf-report/vaccine-hesitancy)                                                                                        | hesitancy              | 756   | 17.677 | 3.798  | 7.000   | 15.000 | 18.000 | 20.000 | 27.000  |
| Proportion of Black Residents            | Proportion of black residents in county i.                                                             | County Population by Characteristics: 2010-2019 (https://www.census.gov/data/tables/time-series/demo/popest/2010s-counties-detail.html)                                                                                                           | Black_Prop             | 756   | 12.397 | 13.136 | 0.541   | 3.103  | 7.429  | 16.913 | 76.973  |
| Flu Vaccination Rate                     | Rate of flu vaccination among Medicare beneficiaries in county i.                                      | CMS Mapping Medical Disparity Tool (https://data.cms.gov/mapping-medicare-disparities)                                                                                                                                                            | FluVax_Rate            | 756   | 47.622 | 7.552  | 19.000  | 43.000 | 49.000 | 53.000 | 65.000  |
| Flu Vaccination Disparity                | Flu vaccination disparity between White and Black Medicare beneficiaries in county i.                  | CMS Mapping Medical Disparity Tool (https://data.cms.gov/mapping-medicare-disparities)                                                                                                                                                            | FluVax_Disparity       | 756   | 15.185 | 6.445  | -12.000 | 12.000 | 16.000 | 19.000 | 37.000  |
| Proportion of Pop. Above Age 75          | Rate of  Age >=75 population in county i.                                                              | County Population by Characteristics: 2010-2019 (https://www.census.gov/data/tables/time-series/demo/popest/2010s-counties-detail.html)                                                                                                           | Above75_Rate           | 756   | 7.680  | 1.844  | 3.515   | 6.469  | 7.697  | 8.758  | 17.853  |
| Above Age 75 Disparity                   | Age >= 75 population disparity between white and black people in county i.                             | County Population by Characteristics: 2010-2019 (https://www.census.gov/data/tables/time-series/demo/popest/2010s-counties-detail.html)                                                                                                           | Above75_Disparity      | 756   | 4.583  | 2.452  | -4.481  | 3.099  | 4.717  | 5.966  | 17.401  |
| Proportion of Pop. Above Age 15 Below 74 | Rate of  15 <= age <= 74 population in county i.                                                       | County Population by Characteristics: 2010-2019 (https://www.census.gov/data/tables/time-series/demo/popest/2010s-counties-detail.html)                                                                                                           | A15T74_Rate            | 756   | 74.389 | 2.144  | 65.922  | 73.190 | 74.299 | 75.436 | 83.149  |
| Age 15 <= Age <= 74 Disparity            | 15 <= age <= 74 population disparity between white and black people in county i.                       | County Population by Characteristics: 2010-2019 (https://www.census.gov/data/tables/time-series/demo/popest/2010s-counties-detail.html)                                                                                                           | A15T74_Disparity       | 756   | -0.433 | 4.311  | -20.894 | -1.974 | 0.064  | 1.996  | 17.110  |
| Test Positivity                          | Rate of Nucleic Acid Amplification Tests (NAATs) positivity in county i.                                | CDC COVID-19 Integrated County View (https://covid.cdc.gov/covid-data-tracker/#county-view)                                                                                                                                                       | Positivity             | 756   | 6.334  | 3.765  | 0.000   | 3.828  | 5.760  | 8.433  | 26.080  |


<a name="correlation_map"/>

### Correlation Matrix Heatmap for Final Data

![](_img/correlation.png)

**Figure S4.** A correlation matrix heatmap illustrate bivariate relationships among all variables in our main regression results and robustness checks.

<a name="main_reg"/>

## Main Regression Result



<a name='base_model'>
### Base Model Regression Table


 
<a name='coef_plot'>
### Coefficient Plot for COVID-19 and Flu Vaccination Disparity Analysis
    
![](_img/coef.png)



<a name="robustness_checks"/>

## Detailed Regression Results and Robustness Checks

    
    
 
<a name="age_control"/>
    
 ### Different Age Group Control
    

    
 
<a name="disparity_measure"/>
    
 ### Different Disparity Measurements
   
    
    
 
<a name="date_ratetype"/>
    
### Different Dates and Different Vaccination Rate Types

    
    

<a name="exodus_test"/>
   
### Exodus
   
   
   
 
<a name="positivity"/>
   
### Recent Positive Rate per COVID-19 Test]

   
