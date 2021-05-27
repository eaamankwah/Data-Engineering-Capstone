# Data-Engineering-Capstone

![pyspark](https://github.com/eaamankwah/Data-Engineering-Capstone/blob/main/screenshots/pyspark_logo.png)

# Table of Contents
* Overview
* Project Dataset
* Star Database Schema Design
* * Staging Tables
* * Fact Table
* * Dimension Tables
*  Project Steps
* * Main Project Steps
* * Project Recommendations
* * Document Process
* References

## Overview
This project is final capstone project of the Udacity Data Engineering Nanodegree. 

The object of this project is to construct a  data warehouse with Apache PySpark, which includes building an ETL pipeline that extracts their data from three different sources, stages them, and transforms data into a set of dimensional tables for their analysis that general data driven insight for the proposed client.

## Project Dataset

The main dataset include data on immigration to the United States, and supplementary datasets which include data on airport codes, U.S. city demographics, and temperature data. 

**I94 Immigration Data**

This data comes from the US National Tourism and Trade Office [immigration data](https://www.trade.gov/national-travel-and-tourism-office) and includes details on incoming immigrants and their ports of entry. 

For example,  below are file path to one file in this dataset.

../../data/18-83510-I94-Data-2016/i94_jun16_sub.sas7bdat

**U.S. City Demographic Data**

comes from OpenSoft [demographic data]( https://public.opendatasoft.com/explore/dataset/us-cities-demographics/export/) and includes data by city, state, age, population, veteran status and race.

**Temperature Data**

This data comes from kaggle [world temperature data](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data) and includes data on temperature changes in the world since 1850. Monthly US temperatures based on cities was extracted from this dataset.

For example,  below are file path to one file in this dataset.

'../../data2/GlobalLandTemperaturesByCity.csv’ 

## Star Schema Design

A star schema was used for this project and has the following benefits:

* Queries are simpler: because all of the data connects through the fact table the multiple dimension tables are treated as one large table of information, and that makes queries simpler and easier to perform.
* Easier business insights reporting: Star schemas simplify the process of pulling business reports like "what songs users are listening to".
* Better-performing queries: by removing the bottlenecks of a highly normalized schema, query speed increases, and the performance of read-only commands improves.
* Provides data to OLAP systems: OLAP (Online Analytical Processing) systems can use star schemas to build OLAP cubes.

The star schema optimized for queries on US immigration analysis includes the tables below:

### Staging Table

Staging tables are permanent tables used to store temporary data before transferring to another permanent table. This seemly intermediate process helps to process data from externa sources before transferring them to the database tables.  Some of the immediate benefits of staging tables includes data cleansing, computing values based on source data, re-shaping and/or re-distributing source data layout to one that matches the needs of a data warehouse.

Advantages of Staging Tables

* Baseline: the staging tables provides buffer that isolates the warehouse from the source systems, and when warehouse processing is interrupted and has to be restarted.
* Traceability: the data sources develop based on operational needs. The staging tables capture source data at the time of each extract and permit strict traceability from user analytics back through to source data.
* Agility: staging allows developers to use SQL to access staging data and test different warehouse structures. While simple SQL codes can be left in place, complex SQL codes added into the ETL process, thereby providing agility in the warehouse development.
* Warehouse evolution: developers can reprocess warehouse data upon restructuring and during agile development
* [Dataversity](https://www.dataversity.net/data-warehouses-stage-source-data/#:~:text=Staging%20tables%20provide%20a%20buffer,data%20from%20their%20operational%20counterparts.)

In this project, two staging tables are provided below:

#### Staging_immig
This table included the follow columns: id, date, city_code, state_code, age, gender, visa_type and count

#### Staging_ustemp
This table included the following columns: year, month, city_code, city_name, avg_temperature, lat and long. 

#### Staging _demog
This table included the following columns: city_code, state_code, city_name, median_age, pct_male_pop, pct_female_pop, pct_veterans
pct_foreign_born, pct_native_american, pct_asian, pct_black, pct_hispanic_or_latino, pct_white and total_pop.

### Fact Table
The main fact table (immigration) contains all the measures associated with the following columns and dimension tables: id, state_code. city_code, date and count.

### Dimension Tables

The dimension tables below contain detailed information about each row in the fact table.

#### Immigrants

This table included the following columns: id, gender, age and visa_type.

#### Immigrants_city

This table included the following columns: city_code, state_code, city_name, median_age, pct_male_pop, pct_female_pop, pct_veterans
pct_foreign_born, pct_native_american, pct_asian, pct_black, pct_hispanic_or_latino, pct_white, total_pop, lat and long.

#### City_monthly_temp
This table included the following columns: city_code, year, month and avg_temperature.

#### Time 
This table included the following columns: date, dayofweek, weekofyear and month

The following screenshot shows the files in the tables were arranged in my Udacity workspace:

![workspace](https://github.com/eaamankwah/Data-Engineering-Capstone/blob/main/screenshots/workspace.png)

## Project Steps

### Main Project Steps

* Perform exploratory data analysis by wrangling the i94 immigration, US  demographics data and the US temperature data, including eliminating duplicate dataset and testing cleaning function.
* Create staging tables to store data before transferring them into permanent tables.
* Create fact table and dimension tables.
* perform prelim ETL steps to load data eventually into the fact and dimension tables
* Write dimension and fact tables to parquet for downstream query
* Perform data quality checks tables are created and not empty.

Note that other specific steps such as the required scoping, data model and data dictionary can be found in the included project Jupyter notebook

### Project Recommendations

**Rationale for the choice of tools and technologies for the project.**

Spark has a Python API, PySpark capable of handling big data with speed due to its in-memory compute technology. Spark can handle different data types such as JSON, Parquet, SAS and CSV and can also scale very well in distributed or parallel environment. Moreover, Spark integrates very well with cloud data stores such as S3 and cloud databases such as Amazon Redshift.

**How often the data should be updated and why.**

The data should be updated on monthly cycles because the format of the raw files is monthly. This cycle will also work for most entities such as organizations and governments.

**How my approach to the problem would differ under the following scenarios:**

**The data was increased by 100x.**
Amazon Redshift would be used since it provides analytical database which is optimized for aggregation and read-heavy workloads

**The data populates a dashboard that must be updated on a daily basis by 7am every day.**
Apache Airflow or Apache NiFi could be used to create DAG pipelines or automate the process that send failures notices or perform retries. Daily quality checks that send failure emails to operators could be employed for consistent monitoring in order to meet users and client requirements.

**The database needed to be accessed by 100+ people.**
The auto scaling capacities of redshift could be used to handle heavy loads and also attain good read performance.


### Document Process

Do the following steps in your README.md file.
1. Discuss the purpose of this data model in context of the immigration to US and their analytical goals.
2. State and justify your data model schema design and ETL pipeline.
3. Provide data quality checks
4. Recommendations

## References

* [Dataversity](https://www.dataversity.net/data-warehouses-stage-source-data/#:~:text=Staging%20tables%20provide%20a%20buffer,data%20from%20their%20operational%20counterparts.)

* [Udacity Q & A Platform](https://knowledge.udacity.com/?nanodegree=nd027&page=1&project=577&rubric=2497)


