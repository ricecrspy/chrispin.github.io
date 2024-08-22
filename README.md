# Data Analyst & Data Visualization

## Technical Skills
#### SQL, Python, Looker Studio, BigQuery, Tableau, Google Sheets, Excel, Adobe Illustrator, Adobe Photoshop, Adobe XD, Adobe Creative Cloud

## Education
IBM Python for Data Science, AI Development Certification | Coursera (2024)
Google Data Analytics Certification | Coursera (2024)
B.A. Fashion Design & Marketing | IADT (2008)

## Work Experience
**Digital Marketing Analyst @ Haute Hero (2014 - 2023)**
- User-generated content and engagement analysis resulting in 30% YoY increased user engagement and 20% YoY follower growth (2015-2022)
- Digital marketing content strategy and implementation; optimization of facebook ads and instagram organic reach achieving a Socialblade.com score of B+ (2018-2022)
- Social media brand collaborations w/ Puma (150k + views), Business Insider (3 million + views), Galieo (16 million + views EU), Hypebeast (48k views), and Sketchers (2023).
- Increased D2C Shopify conversion rates by 15% YoY through UI/UX optimization, user behavior analytics, and sales analysis.

**Data Analyst | Business Intelligence @ Statmask (2020 - 2023)**
- Market trend research leading to a 30% YoY increase in D2C sales by targeting key ad platforms and influencer channels. Web crawling for first party data collection, google sheets/bigQuery/SQL for data cleaning and Looker Studio for KIP reporting.
- Geolocation dashboard using first party shopify sales data to optimize influencer collaborations and ad targeting. 
- Achieved a 150% boost in manufacturing efficiency by analyzing CNC and sewing machine data by identifying production bottlenecks using custom analytical models. 
- Developed a competitive research dashboard analyzing government contracts using secondary federal contracting data helping the company identifies local and national competitors ranked by yearly revenue, location, and product type (NAICS Codes). 

## Projects
### STATMASK

#### 1.0 About
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit.

#### 1.1 Geo-Location Dashboard
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit.
<br><br>
![geo_location](assets/img/portfolio/capstone/statmask_geolocation_dashboard.png)

#### 1.2 Profit and Loss Dashboard
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit.
<br><br>
![profitloss](assets/img/portfolio/capstone/statmask_PnL.png)

### BERLIN OUTERWEAR MARKET REPORT

#### 1.1 OBJECTIVE
Google Data Analytics Capstone: Berlin, a premium streetwear fashion brand, wants to expand by offering outerwear pieces in its collection. However, before launching the new product line, the company's President wants a high-level report analyzing current market trends and competitors. The Director of Marketing and Sales tasked me with performing market research and creating a report that highlights trends, market insights, and key recommendations using data analytics.

#### 1.2 OUTLINE

- 1.3 DATA GATHERING
- 1.4 DATA CLEANING AND MANIPULATION w/ SQL
- 1.5 DATA ANALYSIS
- 1.6 NULL VALUES
- 1.7 VISUALIZATION
- 1.8 REPORT PREVIEW

#### 1.3 DATA GATHERING
I identified five e-commerce boutique stores that align with Berlin's market positioning in terms of price, product design, target customer, and quality. Using a Chrome browser web crawler extension, I scraped over 300 rows of data observations, about 30-70 rows from each website, based on the following variables: brand name, product description, colorway, price, discount, and sale price. Data structures varied from store to store, and as a result, a unique table was created for each, totaling five tables.

![Data_gathering](assets/img/portfolio/capstone/data_gathering.png)

After creating matching columns in all five tables: ```brand_id```, ```description```, ```colorway```, ```sales```, and ```discount``` in google sheets, I was able to aggregate the data using the ```=QUERY()``` function, however this method achieved only surface level organization. As a result I would need to use bigQuery/SQL to properly clean and analyze the data.
<br><br>
![google_sheets_tlb](assets/img/portfolio/capstone/google_sheets_tlbs.png)

#### 1.4 DATA CLEANING AND MANIPULATION w/ SQL

To prepare the data for analysis, I used BigQuery to join all tables into a single dataset under the ```brand_id``` key. I combined ```product_name``` with ```description``` into one description variable, resulting in a cleaned and aggregated dataset. However, to analyze product category frequencies, I needed to extract string-specific data from the description column. Therefore, I created two variables: ```style_id``` and ```sub_style_id```, and extracted them from ```description``` using ```REGEXP_EXTRACT()``` and Common Table Expressions (CTE) for aggregation. The first variable represented broad apparel categories such as jackets, coats, tracks, vests, etc., while the second variable identified specific properties such as materials or functions (e.g., down, shell, windstop, etc.). See the SQL query below.
<br><br>
```
WITH descrip_tbl AS -- replace decription_id CTE
(SELECT
  brand_id,
  product_name,
  REPLACE(product_name, 'Jkt', 'Jacket') AS description_id,
  
FROM data-analytics-course-413120.gda_course_8_data.outerwear_tbl
GROUP BY brand_id, product_name
),
style_tbl AS  -- Style_id CTE
(SELECT
  brand_id,
  product_name,
    REGEXP_EXTRACT(
    description_id,
    r'Jacket|Vest|Parka|Coat|Fleece|Anorak|Overcoat|
      Peacoat|Gilet|Track Top|Sweatshirt|Shacket|Overshirt'
  ) AS style_id
  FROM descrip_tbl
  GROUP BY brand_id, product_name, description_id
),
sub_style_tbl AS -- Sub_style_id CTE`
(SELECT
  brand_id,
  product_name,
    REGEXP_EXTRACT(
    product_name,
    r'Padded|Down|Linner|Puffer|Bomber|Varsity|Hoodie|Flight|Coach|
    Fleece|Shirt|Track|Packable|Nylon|Chore|Shell|Zip|Ripstop|GORE-TEX|
    Packable|Windrunner|Shearling|Quilted|Wool|Twill|Corduroy|Canvas|
    Flannel|WIP|Denim|Windbreaker|Primaloft®|Cotton|Hoodied|Leather'
  ) AS sub_style_id

FROM data-analytics-course-413120.gda_course_8_data.outerwear_tbl
GROUP BY brand_id, product_name
)
SELECT 
  berlin_ds.brand_id,
  descrip_tbl.description_id,
  style_tbl.style_id,
  sub_style_tbl.sub_style_id,
  berlin_ds.price  
FROM 
  data-analytics-course-413120.gda_course_8_data.outerwear_tbl AS berlin_ds 
  FULL OUTER JOIN descrip_tbl ON berlin_ds.product_name = descrip_tbl.product_name
  FULL OUTER JOIN style_tbl ON descrip_tbl.product_name = style_tbl.product_name
  FULL OUTER JOIN sub_style_tbl ON style_tbl.product_name = sub_style_tbl.product_name

GROUP BY brand_id,description_id, style_id, sub_style_tbl.sub_style_id, price
ORDER BY price DESC
```
#### 1.5 DATA ANALYSIS
Below is the final dataset after the description variable was aggregated and the string data for ```style_id``` and ```sub_style_id``` were extracted. The dataset is now ready for analysis.
<br><br>
![clean](assets/img/portfolio/capstone/cleaned_data.png)

#### 1.6 NULL VALUES
The majority of the nulls values in the dataset were in the ```sale_price``` and ```discount``` columns, both of which were not used in the following report.

#### 1.7 VISUALIZATION
In conclusion, I gathered first-party data from potential competitors to create the dataset, cleaned and manipulated the dataset using Google Sheets and BigQuery/SQL, created two new variables for frequency analysis, and visualized the data using Looker Studio in a comprehensive market report. Below is the completed report with key insights and recommendations. [Click here to view full report](https://lookerstudio.google.com/reporting/b7b2a1d2-341f-4405-9743-5b4390291fa3)
<br><br>
#### 1.8 REPORT PREVIEW
<br><br>
![cover_page](assets/img/portfolio/capstone/cover_page_16x9.png)
![categories](assets/img/portfolio/capstone/categories_16x9.png)
![down](assets/img/portfolio/capstone/down_16x9.png)
![unique_styles](assets/img/portfolio/capstone/unique_styles_16x9.png)
![canada](assets/img/portfolio/capstone/canada_goose_16x9.png)
![key_takeaways](assets/img/portfolio/capstone/key_takeaways_16x9.png)





