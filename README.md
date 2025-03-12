# Carbon-Emission-Analysis

## 1. Explore Data
Table 'product_emissions'
```sql
SELECT * FROM product_emissions LIMIT 5;
```
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

Table 'industry_groups'
```sql
SELECT * FROM industry_groups LIMIT 10
```
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 
| 6  | "Textiles, Apparel, Footwear and Luxury Goods"                         | 
| 7  | Automobiles & Components                                               | 
| 8  | Capital Goods                                                          | 
| 9  | Chemicals                                                              | 
| 10 | Commercial & Professional Services                                     | 

Table 'companies'
```sql
SELECT * FROM companies LIMIT 10
```
| id | company_name                                   | 
| -: | ---------------------------------------------: | 
| 1  | "Autodesk, Inc."                               | 
| 2  | "Casio Computer Co., Ltd."                     | 
| 3  | "Cisco Systems, Inc."                          | 
| 4  | "CNX Coal Resources, LP"                       | 
| 5  | "Coca-Cola Enterprises, Inc."                  | 
| 6  | "Compañía Española de Petróleos, S.A.U. CEPSA" | 
| 7  | "Daikin Industries, Ltd."                      | 
| 8  | "Elitegroup computer systems co., Ltd."        | 
| 9  | "Fuji Xerox Co., Ltd."                         | 
| 10 | "Gamesa Corporación Tecnológica, S.A."         | 

Table 'countries'
```sql
SELECT * FROM countries LIMIT 10
```
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 
| 6  | China        | 
| 7  | Colombia     | 
| 8  | Finland      | 
| 9  | France       | 
| 10 | Germany      | 
## 2. Carbon Emission Data Analysis
### 1. Which products contribute the most to carbon emissions?
```sql
SELECT product_name,ROUND(AVG(carbon_footprint_pcf),2) AS "Average PCF"
FROM product_emissions 
GROUP BY product_name
ORDER BY carbon_footprint_pcf DESC
LIMIT 10
```
| product_name                                                                                                                       | Average PCF | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ----------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00  | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00  | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00  | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00  | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00   | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00   | 
| TCDE                                                                                                                               | 99075.00    | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00    | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00    | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00    | 

Conclusion: Wind Turbines emited the most PCF,  Mercedes-Benzs also released considerable PCF into the environment.

### 2. What are the industry groups of these products?
```sql
SELECT industry_group, product_name FROM product_emissions
JOIN industry_groups
ON product_emissions.industry_group_id = industry_groups.id
GROUP BY product_name
ORDER BY AVG(product_emissions.carbon_footprint_pcf) DESC
LIMIT 10;
```
| industry_group                     | product_name                                                                                                                       | 
| ---------------------------------: | ---------------------------------------------------------------------------------------------------------------------------------: | 
| Electrical Equipment and Machinery | Wind Turbine G128 5 Megawats                                                                                                       | 
| Electrical Equipment and Machinery | Wind Turbine G132 5 Megawats                                                                                                       | 
| Electrical Equipment and Machinery | Wind Turbine G114 2 Megawats                                                                                                       | 
| Electrical Equipment and Machinery | Wind Turbine G90 2 Megawats                                                                                                        | 
| Automobiles & Components           | Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 
| Materials                          | Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 
| Materials                          | TCDE                                                                                                                               | 
| Automobiles & Components           | Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 
| Automobiles & Components           | Mercedes-Benz S-Class (S 500)                                                                                                      | 
| Automobiles & Components           | Mercedes-Benz SL (SL 350)                                                                                                          | 

Conclusion: Industry Groups emitting the most PCF focus on Electrical Equipment and Machinery, Automobiles & Components, Materials, Automobiles & Components.

### 3. What are the industries with the highest contribution to carbon emissions?
```sql
SELECT industry_groups.industry_group, ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS "Average PCF"
from industry_groups
JOIN product_emissions
ON product_emissions.industry_group_id = industry_groups.id
GROUP BY industry_groups.industry_group
ORDER BY AVG(product_emissions.carbon_footprint_pcf) DESC
LIMIT 10;
```
| industry_group                                   | Average PCF | 
| -----------------------------------------------: | ----------: | 
| Electrical Equipment and Machinery               | 891050.73   | 
| Automobiles & Components                         | 35373.48    | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.00    | 
| Capital Goods                                    | 7391.77     | 
| Materials                                        | 3208.86     | 
| "Mining - Iron, Aluminum, Other Metals"          | 2727.00     | 
| Energy                                           | 2154.80     | 
| Chemicals                                        | 1949.03     | 
| Media                                            | 1534.47     | 
| Software & Services                              | 1368.94     | 

Conclusion: Electrical Equipment and Machinery is the industry with the highest contribution to carbon emissions. Next, Automobiles & Components holds second position. It could be seen that "Pharmaceuticals, Biotechnology & Life Sciences" released colossal PCF and placed third position. The products with the highest levels of carbon emissions are typically associated with heavy industry.

### 4. What are the companies with the highest contribution to carbon emissions?
```sql
SELECT companies.company_name, ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS "Average PCF" 
FROM companies
JOIN product_emissions
ON product_emissions.company_id = companies.id
GROUP BY company_name
ORDER BY AVG(product_emissions.carbon_footprint_pcf) DESC
LIMIT 10;
```
| company_name                           | Average PCF | 
| -------------------------------------: | ----------: | 
| "Gamesa Corporación Tecnológica, S.A." | 2444616.00  | 
| "Hino Motors, Ltd."                    | 191687.00   | 
| Arcelor Mittal                         | 83503.50    | 
| Weg S/A                                | 53551.67    | 
| Daimler AG                             | 43089.19    | 
| General Motors Company                 | 34251.75    | 
| Volkswagen AG                          | 26238.40    | 
| Waters Corporation                     | 24162.00    | 
| "Daikin Industries, Ltd."              | 17600.00    | 
| CJ Cheiljedang                         | 15802.83    | 

Conclusion: Gamesa Corporación Tecnológica, S.A. is the company with the highest contribution to carbon emissions. After that, Hino Motors, Ltd also emitted a huge amount of PCF into the environment.

### 5. What are the countries with the highest contribution to carbon emissions?
```sql
SELECT countries.country_name, ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS "Average PCF" 
FROM countries
JOIN product_emissions
ON product_emissions.country_id = countries.id
GROUP BY country_name
ORDER BY AVG(product_emissions.carbon_footprint_pcf) DESC
LIMIT 10;
```
| country_name | Average PCF | 
| -----------: | ----------: | 
| Spain        | 699009.29   | 
| Luxembourg   | 83503.50    | 
| Germany      | 33600.37    | 
| Brazil       | 9407.61     | 
| South Korea  | 5665.61     | 
| Japan        | 4600.26     | 
| Netherlands  | 2011.91     | 
| India        | 1535.88     | 
| USA          | 1332.60     | 
| South Africa | 1119.27     | 
Conclusion: Spain is the country with the highest contribution to carbon emissions

### 6. What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT year, carbon_footprint_pcf 
FROM product_emissions
GROUP BY year
ORDER BY year;
```
| year | carbon_footprint_pcf | 
| ---: | -------------------: | 
| 2013 | 73                   | 
| 2014 | 2                    | 
| 2015 | 2                    | 
| 2016 | 10000                | 
| 2017 | 1488                 | 

Conclusion: There appears to be a sharp upward trend in 2016, which could indicate an extraordinary incident or reporting change. After the peak in 2016, the carbon footprint returns to a more moderate but still relatively high level in 2017. Overall, the trend suggests irregularities, possibly due to specific events in 2016 that led to the sudden surge, and a correction or decrease following that spike.

### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
