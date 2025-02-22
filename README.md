# SSA-Energy-Report
## Project Overview
This data analysis focuses on presenting the current state of energy system in Sub-Sahara Africa referred to as SSA.

## Data source
The dataset utilized for this project was collected on February, 2025, and sourced from the World Bank Website. It is available for download via the following web link: <a href='https://data.worldbank.org/'>Data</a>. The data for each of the 48 countries making up SSA was retrieved individually and then combined. Each dataset is available over various formats including csv.

## Tools
- MySQL Workbench 8.0 to create an instance of MySQL an connect it to Tableau
- Jupyter notebook IDE for data preprocessing
- Power BI for data visualization

## Data preprocessing
### Database design
As the data was collected individually, a systematic database was needed to aggregate all pieces of information into a structured and normalized format. A database that complies with third normal form was chosen with the following schema:
- **country Table:**
     This table will store information about the countries being tracked and their corresponding region.

     | Column Name | Data Type | Description  |
     |-------------|----------|------------------------------|
     | country_id  | INT      | Unique identifier for country (pk, not null) |
     | country_name| VARCHAR | Name of the country|
     | region| VARCHAR | Name of region|

   - **year Table:**
     This table will store the years from 1960 to 2023.

     | Column Name | Data Type | Description            |
     |-------------|-----------|------------------------|
     | year_id     | INT       | Unique identifier for year  (pk, not null)|
     | year        | INT       | Year value              |

   - **ssa Table:**
     This table will store the data for each country for each year along with energy and socio-economic attribute.

     | Column Name     | Data Type | Description                               |
     |-----------------|-----------|-------------------------------------------|
     | data_id         | INT       | Unique identifier for country data entry  (pk, not null)|
     | country_id      | INT       | Foreign key to the Country Table, FK|
     | year_id         | INT       | Foreign key to the Year Table , FK|
     | Pop             | BIGINT    | Population count for the year            |
     | Expectancy      | DECIMAL   | Life expectancy for the year              |
     | E_power         | DECIMAL   | Electric power consumption (kWh per capita) |
     | Forest          | DECIMAL   | Forest area (sq. km)|
     | Power_loss      | DECIMAL   | Electric power transmission and distribution losses (% of output)|
     | Electricity_p   | DECIMAL   | Access to electricity (% of population)|
     | REO             | DECIMAL   | Renewable electricity output (% of total electricity output)|
     | GDP_p           | DECIMAL   | GDP per capita (current US$)|
     |Literacy         | DECIMAL   | Literacy rate, adult total (% of people ages 15 and above)|
     |Unempl           | DECIMAL   | Unemployment, total (% of total labor force) (national estimate)|
     |CO2_tr          | DECIMAL   | Carbon dioxide (CO2) emissions from Transport (Energy) (Mt CO2e)|
     |CO2_ind         | DECIMAL   | Carbon dioxide (CO2) emissions from Industrial Combustion (Energy) (Mt CO2e)|
     
  ### Data cleaning and preparation
  The initial csv file for each country had nearly 1500 attributes (Variables) arranged in row. So, the first thing was to transpose the content of each csv file in excel to have the attribute appearing in columns. then each csv files were imported in python. The columns relevant for the study were selected and the number of attributed were thus reduced significantly. To address missing values as it was the case for many countries the methol 'interpolate' of pandas library in python were employed. At the end each csv was cleaned, with no missing values and with only a few attributes. They were then added manually to the ssa table. The detailed preprocessing work in python can be acessed (see file attached).

## Exploration Data Analysis

  
