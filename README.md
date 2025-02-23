# SSA Energy Report

## Project Overview
This data analysis project focuses on presenting the current state of the energy system in Sub-Saharan Africa (SSA). The report provides insights into energy consumption, socio-economic indicators, and environmental impacts across the region.

---

## Data Source
The dataset used for this project was collected in February 2025 and sourced from the World Bank website. It is available for download via the following link: [World Bank Data](https://data.worldbank.org/). Data for each of the 48 countries in SSA was retrieved individually and later combined. The dataset is available in various formats, including CSV.

---

## Tools
The following tools were utilized for this project:
- **MySQL Workbench 8.0**: For database creation and connection to Tableau.
- **Jupyter Notebook**: For data preprocessing and cleaning.
- **Power BI**: For data visualization.

---

## Data Preprocessing

### Database Design
Since the data was collected individually, a systematic database was designed to aggregate all information into a structured and normalized format. A third normal form (3NF) compliant database schema was implemented with the following tables:

#### **Country Table**
This table stores information about the countries and their corresponding regions.

| Column Name   | Data Type | Description                              |
|---------------|-----------|------------------------------------------|
| country_id    | INT       | Unique identifier for the country (Primary Key, Not Null) |
| country_name  | VARCHAR   | Name of the country                      |
| region        | VARCHAR   | Name of the region                       |

#### **Year Table**
This table stores the years from 1960 to 2023.

| Column Name | Data Type | Description                              |
|-------------|-----------|------------------------------------------|
| year_id     | INT       | Unique identifier for the year (Primary Key, Not Null) |
| year        | INT       | Year value                               |

#### **SSA Table**
This table stores energy and socio-economic data for each country and year.

| Column Name       | Data Type | Description                              |
|-------------------|-----------|------------------------------------------|
| data_id           | INT       | Unique identifier for the data entry (Primary Key, Not Null) |
| country_id        | INT       | Foreign key to the Country Table         |
| year_id           | INT       | Foreign key to the Year Table            |
| Pop               | BIGINT    | Population count for the year            |
| Expectancy        | DECIMAL   | Life expectancy for the year             |
| E_power           | DECIMAL   | Electric power consumption (kWh per capita) |
| Forest            | DECIMAL   | Forest area (sq. km)                     |
| Power_loss        | DECIMAL   | Electric power transmission and distribution losses (% of output) |
| Electricity_p     | DECIMAL   | Access to electricity (% of population)  |
| REO               | DECIMAL   | Renewable electricity output (% of total electricity output) |
| GDP_p             | DECIMAL   | GDP per capita (current US$)             |
| Literacy          | DECIMAL   | Literacy rate, adult total (% of people ages 15 and above) |
| Unempl            | DECIMAL   | Unemployment, total (% of total labor force) |
| CO2_tr            | DECIMAL   | CO2 emissions from Transport (Energy) (Mt CO2e) |
| CO2_ind           | DECIMAL   | CO2 emissions from Industrial Combustion (Energy) (Mt CO2e) |

### Data Cleaning and Preparation
The initial CSV files for each country contained nearly 1,500 attributes arranged in rows. The first step was to transpose the content of each file in Excel to organize attributes into columns. Each CSV file was then imported into Python for further processing. Relevant columns were selected, significantly reducing the number of attributes. Missing values, which were prevalent for many countries, were addressed using the `interpolate` method from the Pandas library. After cleaning, each CSV file was manually added to the `ssa` table. Detailed preprocessing steps in Python can be accessed in the attached file.

---

## Exploratory Data Analysis (EDA)
Several questions were answered using MySQL Workbench. Below are the SQL statements used:

### Data Preprocessing
SET SQL_SAFE_UPDATES = 0;

UPDATE year
SET year = CAST(CONCAT(year, '-01-01') AS DATE)
WHERE year IS NOT NULL;

ALTER TABLE year
MODIFY COLUMN year DATE;

DESCRIBE year;

ALTER TABLE ssa
ADD COLUMN CO2 DOUBLE;

UPDATE ssa
SET CO2 = (CO2_tr + CO2_ind);

Questions Answered

GDP Ranking by Country
SELECT country_name, 
       ROUND(AVG(GDP_p)) AS Avg_GDP
FROM ssa S  
LEFT OUTER JOIN country C
ON S.country_id = C.country_id
GROUP BY country_name
ORDER BY Avg_GDP DESC
LIMIT 10;

GDP Ranking by Region
SELECT region, 
       ROUND(AVG(GDP_p)) AS Avg_GDP
FROM ssa S  
LEFT OUTER JOIN country C
ON S.country_id = C.country_id
GROUP BY region
ORDER BY Avg_GDP DESC;

Electric Power Consumption by Country
SELECT country_name, 
       ROUND(AVG(E_power)) AS Avg_power
FROM ssa S  
LEFT OUTER JOIN country C
ON S.country_id = C.country_id
GROUP BY country_name
ORDER BY Avg_power DESC
LIMIT 10;

Electric Power Consumption by Region
SELECT region, 
       ROUND(AVG(E_power)) AS Avg_power
FROM ssa S  
LEFT OUTER JOIN country C
ON S.country_id = C.country_id
GROUP BY region
ORDER BY Avg_power DESC;

CO2 Emissions by Region
SELECT region, 
       ROUND(AVG(CO2)) AS Avg_CO2
FROM ssa S  
LEFT OUTER JOIN country C
ON S.country_id = C.country_id
GROUP BY region
ORDER BY Avg_CO2 DESC;

CO2 Emissions by Country
SELECT country_name, 
       ROUND(AVG(CO2)) AS Avg_CO2
FROM ssa S  
LEFT OUTER JOIN country C
ON S.country_id = C.country_id
GROUP BY country_name
ORDER BY Avg_CO2 DESC
LIMIT 10;

## Data Analysis
The data analysis was performed using Tableau. Please refer to the uploaded file for detailed visualizations.

## Results

Seven interactive visualizations were created, incorporating five key metrics (GDP per capita, electric power consumption, CO2 emissions, energy access, and population) and three filters (years, country names, and regions). Key findings include:

- GDP per capita ranged from 160to160to6,768 between 1960 and 2023.

- Electric power consumption per capita ranged from 37 kWh to 3,575 kWh during the same period.

- CO2 emissions varied significantly, ranging from nearly 4% to 68%.

- South Africa is the top CO2 emitter in SSA, contributing 85.31 Mt of CO2e, followed by Nigeria with 26.02 Mt CO2e.

- Renewable energy constitutes up to 57% of the energy mix in SSA.

- Power losses remain high in SSA, reaching up to 10% in 2023.

Significant efforts are required to improve energy access, which is essential for enabling better access to modern life services such as education, healthcare, and cooking. Initiatives and national programs promoting renewable energy should be encouraged to enhance the adoption and distribution of renewable energy sources. Energy is fundamental to our daily lives, and governments must prioritize increasing generation capacity to revitalize economies and address critical social issues, such as unemployment.
