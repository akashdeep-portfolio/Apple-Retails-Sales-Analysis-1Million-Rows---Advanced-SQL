
# ![Apple Logo](https://github.com/akashdeep-portfolio/Apple_Retails_SalesAnalysis_1MillionRows-SQL_Advance/blob/main/Apple_Retail.jpg)

# Apple Retail Sales SQL Analysis

## Table of Contents
1. [Project Overview](#project-overview)
2. [Entity Relationship Diagram (ERD)](#entity-relationship-diagram-erd)
3. [Dataset & Schema](#dataset--schema)
4. [Optimizing with Indexing](#optimizing-with-indexing)
   - [Performance Issue](#performance-issue)
   - [Applying Index](#applying-index)
5. [Objectives](#objectives)
   - [Easy to Medium](#easy-to-medium)
   - [Medium to Hard](#medium-to-hard)
   - [Complex](#complex)
6. [Bonus Question](#bonus-question)
7. [Project Focus](#project-focus)
8. [Conclusion](#conclusion)


## Project Overview

This project showcases advanced SQL querying techniques by analyzing over 1 million rows of Apple retail sales data, including products, stores, transactions, and warranty claims. It focuses on query optimization, trend analysis, and business insights using window functions, joins, and performance tuning. The project demonstrates the ability to work with large-scale datasets and extract real-world business insights.

## Entity Relationship Diagram (ERD)

![ERD](https://github.com/akashdeep-portfolio/Apple_Retails_SalesAnalysis_1MillionRows-SQL_Advance/blob/main/erd.png)

---
## Dataset & Schema - [Download Dataset](https://github.com/akashdeep-portfolio/Apple_Retails_SalesAnalysis_1MillionRows-SQL_Advance/blob/main/apple_datasets.zip)

- **Size**: 1 million+ rows of sales data.
- **Period Covered**: The data spans multiple years, allowing for long-term trend analysis.
- **Geographical Coverage**: Sales data from Apple stores across various countries.

**The project uses five main tables**:
1. **stores**: Contains information about Apple retail stores.
   - `store_id`: Unique identifier for each store.
   - `store_name`: Name of the store.
   - `city`: City where the store is located.
   - `country`: Country of the store.

2. **category**: Holds product category information.
   - `category_id`: Unique identifier for each product category.
   - `category_name`: Name of the category.

3. **products**: Details about Apple products.
   - `product_id`: Unique identifier for each product.
   - `product_name`: Name of the product.
   - `category_id`: References the category table.
   - `launch_date`: Date when the product was launched.
   - `price`: Price of the product.

4. **sales**: Stores sales transactions.
   - `sale_id`: Unique identifier for each sale.
   - `sale_date`: Date of the sale.
   - `store_id`: References the store table.
   - `product_id`: References the product table.
   - `quantity`: Number of units sold.

5. **warranty**: Contains information about warranty claims.
   - `claim_id`: Unique identifier for each warranty claim.
   - `claim_date`: Date the claim was made.
   - `sale_id`: References the sales table.
   - `repair_status`: Status of the warranty claim (e.g., Paid Repaired, Warranty Void).

## Optimizing with Indexing

To improve the efficiency of queries in the Apple Retail Sales SQL Project, indexing was applied to key columns. One of the primary challenges was the slow execution of queries filtering sales data based on sale date. Since the `sales` table contains over 1 million rows, a full table scan led to poor performance.

### Performance Issue
Initially, queries filtering sales by date were slow:
```sql
EXPLAIN ANALYZE
SELECT * FROM sales
WHERE sale_date >= '2023-01-01';
```
- **Execution Time Before Indexing**: **~64ms** (Full Table Scan)
- **Cause**: The database had to scan all 1M+ rows, making the query inefficient.

### Applying Index
To enhance query performance, an index was created on the `sale_date` column:
```sql
CREATE INDEX idx_sale_date ON sales(sale_date);
```
- **Purpose**: Allows the database to quickly locate sales based on date rather than scanning all rows.
- **Benefit**: Significantly reduces query execution time.

## Objectives

The project is split into three tiers of questions to test SQL skills of increasing complexity:

### Easy to Medium

1. Find the number of stores in each country.
```sql
SELECT 
	country,
	COUNT(store_id) as total_stores
FROM stores
GROUP BY 1
ORDER BY 2 DESC
```
<details>
  <summary>Data Output</summary>

| Country               | Total Stores |
|-----------------------|-------------|
| UK                   | 10          |
| USA                  | 10          |
| Canada               | 8           |
| China                | 4           |
| Australia            | 3           |
| Brazil               | 2           |
| Japan                | 2           |
| Spain                | 2           |
| Turkey               | 2           |
| India                | 2           |
| France               | 2           |
| Germany              | 2           |
| UAE                  | 2           |
| Argentina            | 1           |
| Chile                | 1           |
| Vietnam              | 1           |
| Mexico               | 1           |
| Peru                 | 1           |
| Malaysia             | 1           |
| Taiwan               | 1           |
| Costa Rica           | 1           |
| Philippines          | 1           |
| Thailand             | 1           |
| Indonesia            | 1           |
| Switzerland          | 1           |
| Italy                | 1           |
| Venezuela            | 1           |
| Uruguay              | 1           |
| Qatar                | 1           |
| Netherlands          | 1           |
| Dominican Republic   | 1           |
| Bolivia              | 1           |
| Singapore            | 1           |
| South Korea          | 1           |
| Saudi Arabia         | 1           |

</details><br>

2. Calculate the total number of units sold by each store.
```sql
SELECT 
	s.store_id,
	st.store_name,
	SUM(s.quantity) as total_unit_sold
FROM sales as s
JOIN
stores as st
ON st.store_id = s.store_id
GROUP BY 1, 2
ORDER BY 3 DESC
```
<details>
  <summary>Data Output</summary>

| Store ID  | Store Name                         | Total Units Sold |
|-----------|-----------------------------------|------------------|
| ST-5      | Apple South Coast Plaza          | 47498           |
| ST-3      | Apple Michigan Avenue            | 44891           |
| ST-4      | Apple The Grove                  | 44784           |
| ST-2      | Apple Union Square               | 44395           |
| ST-1      | Apple Fifth Avenue               | 44367           |
| ST-12     | Apple Yorkdale                   | 29882           |
| ST-14     | Apple Pacific Centre             | 29595           |
| ST-9      | Apple Lennox Town Center         | 29536           |
| ST-6      | Apple Scottsdale                 | 29504           |
| ST-8      | Apple Eastview Mall              | 29451           |
| ST-13     | Apple Square One                 | 29357           |
| ST-11     | Apple Eaton Centre               | 29297           |
| ST-10     | Apple Tysons Corner              | 29215           |
| ST-7      | Apple Mall of America            | 29196           |
| ST-53     | Apple New Delhi                  | 27464           |
| ST-52     | Apple Mumbai                     | 27455           |
| ST-51     | Apple Seoul                      | 27406           |
| ST-54     | Apple Ankara                     | 27229           |
| ST-55     | Apple Istanbul                   | 27026           |
| ST-21     | Apple Orchard Road               | 23094           |
| ST-23     | Apple Covent Garden              | 23036           |
| ST-22     | Apple Regent Street              | 23030           |
| ST-24     | Apple Bluewater                  | 22906           |
| ST-25     | Apple Munich                     | 22888           |
| ST-18     | Apple CF Sherway Gardens         | 19351           |
| ST-20     | Apple Shibuya                    | 19329           |
| ST-16     | Apple Rideau Centre              | 19329           |
| ST-19     | Apple Marunouchi                 | 19202           |
| ST-15     | Apple Chinook Centre             | 19174           |
| ST-17     | Apple West Edmonton Mall         | 19143           |
| ST-27     | Apple Amsterdam                  | 14863           |
| ST-29     | Apple Lyon                       | 14733           |
| ST-26     | Apple Kurfürstendamm             | 14668           |
| ST-30     | Apple Milan                      | 14631           |
| ST-28     | Apple Champs-Élysées             | 14485           |
| ST-38     | Apple Brisbane                   | 12196           |
| ST-36     | Apple Sydney                     | 12120           |
| ST-34     | Apple Madrid                     | 12060           |
| ST-35     | Apple Zurich                     | 12056           |
| ST-41     | Apple Mexico City                | 12006           |
| ST-40     | Apple Rio de Janeiro             | 11995           |
| ST-37     | Apple Melbourne                  | 11980           |
| ST-39     | Apple São Paulo                  | 11978           |
| ST-31     | Apple Dubai Mall                 | 11334           |
| ST-33     | Apple Barcelona                  | 9918            |
| ST-50     | Apple Taipei                     | 8270            |
| ST-44     | Apple Kuala Lumpur               | 8232            |
| ST-43     | Apple Jakarta                    | 8173            |
| ST-49     | Apple Chengdu                    | 8154            |
| ST-42     | Apple Santiago                   | 8140            |
| ST-45     | Apple Bangkok                    | 8125            |
| ST-48     | Apple Shanghai                   | 8122            |
| ST-47     | Apple Beijing                    | 8113            |
| ST-46     | Apple Hong Kong                  | 8071            |
| ST-32     | Apple Mall of the Emirates       | 6453            |
| ST-62     | Apple Lima                       | 5800            |
| ST-64     | Apple San José                   | 5632            |
| ST-61     | Apple Caracas                    | 5610            |
| ST-63     | Apple Montevideo                 | 5606            |
| ST-65     | Apple La Paz                     | 5586            |
| ST-58     | Apple Manila                     | 5578            |
| ST-56     | Apple Doha                       | 5541            |
| ST-57     | Apple Riyadh                     | 5498            |
| ST-60     | Apple Buenos Aires               | 5489            |
| ST-59     | Apple Ho Chi Minh City           | 5424            |
| ST-68     | Apple Manchester                 | 714             |
| ST-66     | Apple Santo Domingo              | 709             |
| ST-70     | Apple Newcastle                  | 685             |
| ST-67     | Apple Birmingham                 | 678             |
| ST-69     | Apple Sheffield                  | 647             |

</details><br>

3. Identify how many sales occurred in December 2023.
```sql
SELECT 
	COUNT(sale_id) as total_sale 
FROM sales
WHERE TO_CHAR(sale_date, 'MM-YYYY') = '12-2023'
```
<details>
  <summary>Data Output</summary>

| Metric      | Value  |
|------------|--------|
| Total Sale | 32,846 |

</details><br>

4. Determine how many stores have never had a warranty claim filed.

```sql
SELECT COUNT(*) FROM stores
WHERE store_id NOT IN (
						SELECT 
							DISTINCT store_id
						FROM sales as s
						RIGHT JOIN warranty as w
						ON s.sale_id = w.sale_id
						);
```
<details>
  <summary>Data Output</summary>

| Metric | Value |
|--------|-------|
| Count  | 58    |

</details><br>

5. Calculate the percentage of warranty claims marked as "Warranty Void".
```sql
SELECT 
	ROUND
		(COUNT(claim_id)/
						(SELECT COUNT(*) FROM warranty)::numeric 
		* 100, 
	2)as warranty_void_percentage
FROM warranty
WHERE repair_status = 'Warranty Void'
```
<details>
  <summary>Data Output</summary>

| Metric | Value |
|--------|-------|
| Count  | 58    |

</details><br>

6. Identify which store had the highest total units sold in the last year.
```sql
SELECT 
	s.store_id,
	st.store_name,
	SUM(s.quantity)
FROM sales as s
JOIN stores as st
ON s.store_id = st.store_id
WHERE sale_date >= (CURRENT_DATE - INTERVAL '1 year')
GROUP BY 1, 2
ORDER BY 3 DESC
LIMIT 1
```
<details>
  <summary>Data Output</summary>

| Store ID  | Store Name     | Total Sales |
|-----------|---------------|-------------|
| ST-54     | Apple Ankara  | 246         |

</details><br>

7. Count the number of unique products sold in the last year.
```sql
SELECT 
	COUNT(DISTINCT product_id)
FROM sales
WHERE sale_date >= (CURRENT_DATE - INTERVAL '1 year')
```
<details>
  <summary>Data Output</summary>

| Metric | Value |
|--------|-------|
| Count  | 3     |

</details><br>

8. Find the average price of products in each category.
```sql
SELECT 
	p.category_id,
	c.category_name,
	AVG(p.price) as avg_price
FROM products as p
JOIN 
category as c
ON p.category_id = c.category_id
GROUP BY 1, 2
ORDER BY 3 DESC
```
<details>
  <summary>Data Output</summary>

| Category ID | Category Name          | Average Price ($) |
|------------|------------------------|-------------------|
| CAT-1      | Laptop                 | 1511.50          |
| CAT-7      | Desktop                 | 1199.00          |
| CAT-4      | Smartphone              | 889.43           |
| CAT-3      | Tablet                  | 656.50           |
| CAT-5      | Wearable                | 362.33           |
| CAT-2      | Audio                   | 259.00           |
| CAT-6      | Streaming Device        | 179.00           |
| CAT-8      | Subscription Service    | 149.50           |
| CAT-9      | Smart Speaker           | 99.00            |
| CAT-10     | Accessory               | 29.00            |

</details><br>

9. How many warranty claims were filed in 2020?
```sql
SELECT 
	COUNT(*) as warranty_claim
FROM warranty
WHERE EXTRACT(YEAR FROM claim_date) = 2020
```
<details>
  <summary>Data Output</summary>

| Metric          | Value |
|----------------|-------|
| Warranty Claims | 2750  |

</details><br>

10. For each store, identify the best-selling day based on highest quantity sold.
```sql
SELECT  * 
FROM
(
	SELECT 
		store_id,
		TO_CHAR(sale_date, 'Day') as day_name,
		SUM(quantity) as total_unit_sold,
		RANK() OVER(PARTITION BY store_id ORDER BY SUM(quantity) DESC) as rank
	FROM sales
	GROUP BY 1, 2
) as t1
WHERE rank = 1
```
<details>
  <summary>Data Output</summary>

| Store ID  | Best Day  | Total Units Sold | Rank |
|-----------|----------|------------------|------|
| ST-1      | Thursday | 6830             | 1    |
| ST-10     | Sunday   | 4432             | 1    |
| ST-11     | Monday   | 4314             | 1    |
| ST-12     | Sunday   | 4539             | 1    |
| ST-13     | Monday   | 4400             | 1    |
| ST-14     | Thursday | 4366             | 1    |
| ST-15     | Monday   | 2879             | 1    |
| ST-16     | Monday   | 2940             | 1    |
| ST-17     | Thursday | 2837             | 1    |
| ST-18     | Sunday   | 2865             | 1    |
| ST-19     | Monday   | 2985             | 1    |
| ST-2      | Thursday | 6614             | 1    |
| ST-20     | Sunday   | 2864             | 1    |
| ST-21     | Monday   | 3522             | 1    |
| ST-22     | Monday   | 3388             | 1    |
| ST-23     | Monday   | 3416             | 1    |
| ST-24     | Tuesday  | 3443             | 1    |
| ST-25     | Friday   | 3403             | 1    |
| ST-26     | Sunday   | 2219             | 1    |
| ST-27     | Sunday   | 2208             | 1    |
| ST-28     | Thursday | 2204             | 1    |
| ST-29     | Thursday | 2239             | 1    |
| ST-3      | Thursday | 6759             | 1    |
| ST-30     | Sunday   | 2200             | 1    |
| ST-31     | Sunday   | 1731             | 1    |
| ST-32     | Thursday | 1028             | 1    |
| ST-33     | Sunday   | 1567             | 1    |
| ST-34     | Sunday   | 1835             | 1    |
| ST-35     | Tuesday  | 1818             | 1    |
| ST-36     | Tuesday  | 1812             | 1    |
| ST-37     | Sunday   | 1768             | 1    |
| ST-38     | Tuesday  | 1919             | 1    |
| ST-39     | Monday   | 1822             | 1    |
| ST-4      | Thursday | 6807             | 1    |
| ST-40     | Sunday   | 1762             | 1    |
| ST-41     | Thursday | 1827             | 1    |
| ST-42     | Tuesday  | 1246             | 1    |
| ST-43     | Sunday   | 1288             | 1    |
| ST-44     | Tuesday  | 1264             | 1    |
| ST-45     | Monday   | 1238             | 1    |
| ST-46     | Thursday | 1258             | 1    |
| ST-47     | Tuesday  | 1216             | 1    |
| ST-48     | Tuesday  | 1235             | 1    |
| ST-49     | Tuesday  | 1240             | 1    |
| ST-5      | Thursday | 7221             | 1    |
| ST-50     | Sunday   | 1258             | 1    |
| ST-51     | Sunday   | 4169             | 1    |
| ST-52     | Sunday   | 4234             | 1    |
| ST-53     | Sunday   | 4186             | 1    |
| ST-54     | Sunday   | 4161             | 1    |
| ST-55     | Sunday   | 4167             | 1    |
| ST-56     | Tuesday  | 857              | 1    |
| ST-57     | Sunday   | 867              | 1    |
| ST-58     | Tuesday  | 884              | 1    |
| ST-59     | Sunday   | 835              | 1    |
| ST-6      | Thursday | 4355             | 1    |
| ST-60     | Thursday | 845              | 1    |
| ST-61     | Thursday | 873              | 1    |
| ST-62     | Tuesday  | 921              | 1    |
| ST-63     | Tuesday  | 960              | 1    |
| ST-64     | Tuesday  | 907              | 1    |
| ST-65     | Tuesday  | 865              | 1    |
| ST-66     | Monday   | 112              | 1    |
| ST-67     | Thursday | 114              | 1    |
| ST-68     | Tuesday  | 110              | 1    |
| ST-69     | Friday   | 101              | 1    |
| ST-7      | Thursday | 4307             | 1    |
| ST-70     | Sunday   | 111              | 1    |
| ST-8      | Thursday | 4377             | 1    |
| ST-9      | Sunday   | 4462             | 1    |

</details><br>


### Medium to Hard

11. Identify the least selling product in each country for each year based on total units sold.
```sql
WITH product_rank
AS
(
SELECT 
	st.country,
	p.product_name,
	SUM(s.quantity) as total_qty_sold,
	RANK() OVER(PARTITION BY st.country ORDER BY SUM(s.quantity)) as rank
FROM sales as s
JOIN 
stores as st
ON s.store_id = st.store_id
JOIN
products as p
ON s.product_id = p.product_id
GROUP BY 1, 2
)
SELECT 
* 
FROM product_rank
WHERE rank = 1
```
<details>
  <summary>Data Output</summary>

| Country             | Product Name                      | Total Quantity Sold | Rank |
|---------------------|----------------------------------|---------------------|------|
| Argentina          | iPhone 13 Mini                   | 40                  | 1    |
| Australia         | iPhone 12 Mini                   | 106                 | 1    |
| Bolivia           | iPhone 13 Mini                   | 40                  | 1    |
| Brazil            | iPhone 12 Pro Max                | 58                  | 1    |
| Canada            | Mac mini (M1)                    | 824                 | 1    |
| Chile             | Mac mini (M1)                    | 26                  | 1    |
| China             | iPad Air (4th Gen)               | 135                 | 1    |
| Costa Rica        | iPhone 13 Mini                   | 41                  | 1    |
| Dominican Republic | iPhone 15 Pro                    | 225                 | 1    |
| France            | iPhone 12 Mini                   | 66                  | 1    |
| Germany           | Apple Fitness+                   | 129                 | 1    |
| India             | iPad Pro (M2, 11-inch)           | 615                 | 1    |
| Indonesia         | iPad Air (4th Gen)               | 28                  | 1    |
| Italy             | iPad Air (4th Gen)               | 30                  | 1    |
| Italy             | HomePod mini                     | 30                  | 1    |
| Japan             | AirPods Max                      | 193                 | 1    |
| Malaysia         | MacBook Air (M1)                 | 25                  | 1    |
| Mexico            | HomePod mini                     | 29                  | 1    |
| Netherlands       | MacBook Pro (M1, 13-inch)        | 35                  | 1    |
| Peru              | iPhone 13 Mini                   | 45                  | 1    |
| Philippines       | iPhone 13 Mini                   | 43                  | 1    |
| Qatar             | iPhone 13 Mini                   | 45                  | 1    |
| Saudi Arabia      | iPhone 13 Mini                   | 50                  | 1    |
| Singapore         | iPhone 8                         | 86                  | 1    |
| South Korea       | iPad (10th Gen)                  | 312                 | 1    |
| Spain            | AirPods Max                       | 47                  | 1    |
| Switzerland       | Mac mini (M1)                    | 29                  | 1    |
| Taiwan            | iPad Air (4th Gen)               | 32                  | 1    |
| Thailand         | HomePod mini                      | 29                  | 1    |
| Turkey           | iPad Pro (M2, 11-inch)           | 601                 | 1    |
| UAE              | Apple Fitness+                   | 56                  | 1    |
| UK               | iPhone 8 Plus                    | 266                 | 1    |
| Uruguay          | iPhone 13 Mini                   | 53                  | 1    |
| USA              | iPhone 12 Mini                   | 1025                | 1    |
| Venezuela        | iPhone 13 Mini                   | 57                  | 1    |
| Vietnam          | iPhone 13 Mini                   | 50                  | 1    |

</details><br>

12. Calculate how many warranty claims were filed within 180 days of a product sale.
```sql
SELECT 
	COUNT(*)
FROM warranty as w
LEFT JOIN 
sales as s
ON s.sale_id = w.sale_id
WHERE 
	w.claim_date - sale_date <= 180
```
<details>
  <summary>Data Output</summary>

| Metric | Value  |
|--------|--------|
| Count  | 19907  |

</details><br>

13. Determine how many warranty claims were filed for products launched in the last two years.
```sql
SELECT 
	p.product_name,
	COUNT(w.claim_id) as no_claim,
	COUNT(s.sale_id)
FROM warranty as w
RIGHT JOIN
sales as s 
ON s.sale_id = w.sale_id
JOIN products as p
ON p.product_id = s.product_id
WHERE p.launch_date >= CURRENT_DATE - INTERVAL '2 years'
GROUP BY 1
HAVING COUNT(w.claim_id) > 0
```
<details>
  <summary>Data Output</summary>

| Product Name         | No. of Claims | Total Count |
|----------------------|--------------|------------|
| iPhone 15           | 321          | 21,547     |
| iPhone 15 Pro       | 290          | 21,861     |
| iPhone 15 Pro Max   | 320          | 21,661     |

</details><br>

14. List the months in the last three years where sales exceeded 5,000 units in the USA.
```sql
SELECT 
	TO_CHAR(sale_date, 'MM-YYYY') as month,
	SUM(s.quantity) as total_unit_sold
FROM sales as s
JOIN 
stores as st
ON s.store_id = st.store_id
WHERE 
	st.country = 'USA'
	AND
	s.sale_date >= CURRENT_DATE - INTERVAL '3 year'
GROUP BY 1
HAVING SUM(s.quantity) > 5000
```
<details>
  <summary>Data Output</summary>

| Month   | Total Units Sold |
|---------|------------------|
| 03-2022 | 5,831            |
| 04-2022 | 5,779            |
| 09-2022 | 9,789            |
| 10-2022 | 11,005           |
| 11-2022 | 11,289           |
| 12-2022 | 10,724           |

</details><br>

15. Identify the product category with the most warranty claims filed in the last two years.
```sql
SELECT 
	c.category_name,
	COUNT(w.claim_id) as total_claims
FROM warranty as w
LEFT JOIN
sales as s
ON w.sale_id = s.sale_id
JOIN products as p
ON p.product_id = s.product_id
JOIN 
category as c
ON c.category_id = p.category_id
WHERE 
	w.claim_date >= CURRENT_DATE - INTERVAL '2 year'
GROUP BY 1
```
<details>
  <summary>Data Output</summary>

| Category Name         | Total Claims |
|----------------------|--------------|
| Accessory           | 15           |
| Audio              | 673          |
| Laptop             | 32           |
| Smartphone         | 8,507        |
| Subscription Service | 940         |
| Tablet            | 2,966        |
| Wearable          | 972          |

</details><br>

### Complex

16. Determine the percentage chance of receiving warranty claims after each purchase for each country.
```sql
SELECT 
	country,
	total_unit_sold,
	total_claim,
	COALESCE(total_claim::numeric/total_unit_sold::numeric * 100, 0)
	as risk
FROM
(SELECT 
	st.country,
	SUM(s.quantity) as total_unit_sold,
	COUNT(w.claim_id) as total_claim
FROM sales as s
JOIN stores as st
ON s.store_id = st.store_id
LEFT JOIN 
warranty as w
ON w.sale_id = s.sale_id
GROUP BY 1) t1
ORDER BY 4 DESC
```
<details>
  <summary>Data Output</summary>

| Country             | Total Units Sold | Total Claims | Risk (%)           |
|---------------------|-----------------|--------------|---------------------|
| UAE                | 17,939           | 11,816       | 65.87              |
| Spain              | 21,978           | 6,052        | 27.54              |
| Italy              | 14,631           | 1,691        | 11.56              |
| Turkey             | 54,255           | 6,157        | 11.35              |
| India              | 54,919           | 2,241        | 4.08               |
| UK                 | 71,720           | 1,740        | 2.43               |
| Germany            | 37,573           | 641          | 1.71               |
| France             | 29,218           | 335          | 1.15               |
| Netherlands        | 14,863           | 163          | 1.10               |
| Indonesia          | 8,173            | 0            | 0.00               |
| Japan             | 38,531           | 0            | 0.00               |
| Malaysia          | 8,232            | 0            | 0.00               |
| Mexico            | 12,006           | 0            | 0.00               |
| Peru              | 5,800            | 0            | 0.00               |
| Philippines       | 5,578            | 0            | 0.00               |
| Qatar             | 5,541            | 0            | 0.00               |
| Saudi Arabia      | 5,498            | 0            | 0.00               |
| Singapore         | 23,094           | 0            | 0.00               |
| South Korea       | 27,406           | 0            | 0.00               |
| Switzerland       | 12,056           | 0            | 0.00               |
| Taiwan            | 8,270            | 0            | 0.00               |
| Thailand          | 8,125            | 0            | 0.00               |
| Uruguay           | 5,606            | 0            | 0.00               |
| USA               | 372,837          | 0            | 0.00               |
| Venezuela         | 5,610            | 0            | 0.00               |
| Argentina         | 5,489            | 0            | 0.00               |
| Vietnam           | 5,424            | 0            | 0.00               |
| Australia         | 36,296           | 0            | 0.00               |
| Bolivia          | 5,586            | 0            | 0.00               |
| Brazil           | 23,973           | 0            | 0.00               |
| Canada           | 195,128          | 0            | 0.00               |
| Chile            | 8,140            | 0            | 0.00               |
| China            | 32,460           | 0            | 0.00               |
| Costa Rica       | 5,632            | 0            | 0.00               |
| Dominican Republic | 709            | 0            | 0.00               |

</details><br>

17. Analyze the year-by-year growth ratio for each store.
```sql
-- Step 1: Calculate yearly sales for each store
WITH yearly_sales AS (
    SELECT 
        s.store_id,
        st.store_name,
        EXTRACT(YEAR FROM sale_date) AS year,
        SUM(s.quantity * p.price) AS total_sale
    FROM sales AS s
    JOIN products AS p ON s.product_id = p.product_id
    JOIN stores AS st ON st.store_id = s.store_id
    GROUP BY 1, 2, 3
    ORDER BY 2, 3
),

-- Step 2: Calculate sales growth ratio using window function
growth_ratio AS (
    SELECT 
        store_name,
        year,
        LAG(total_sale, 1) OVER(PARTITION BY store_name ORDER BY year) AS last_year_sale,
        total_sale AS current_year_sale
    FROM yearly_sales
)

-- Step 3: Compute the percentage growth ratio
SELECT 
    store_name,
    year,
    last_year_sale,
    current_year_sale,
    ROUND(
        (current_year_sale - last_year_sale)::numeric / last_year_sale::numeric * 100, 3
    ) AS growth_ratio
FROM growth_ratio
WHERE 
    last_year_sale IS NOT NULL
    AND year <> EXTRACT(YEAR FROM CURRENT_DATE);
```
<details>
  <summary>Data Output</summary>

| store_name                 | year | last_year_sale | current_year_sale | growth_ratio |
|----------------------------|------|----------------|-------------------|--------------|
| Apple Amsterdam            | 2020 | 1938674        | 2057521           | 6.13         |
| Apple Amsterdam            | 2021 | 2057521        | 3011823           | 46.381       |
| Apple Amsterdam            | 2022 | 3011823        | 3580638           | 18.886       |
| Apple Amsterdam            | 2023 | 3580638        | 1454241           | -59.386      |
| Apple Amsterdam            | 2024 | 1454241        | 186817            | -87.154      |
| Apple Ankara               | 2021 | 1898618        | 2451685           | 29.13        |
| Apple Ankara               | 2022 | 2451685        | 2355457           | -3.925       |
| Apple Ankara               | 2023 | 2355457        | 15821752          | 571.706      |
| Apple Ankara               | 2024 | 15821752       | 287913            | -98.18       |
| Apple Bangkok              | 2021 | 1843576        | 2427853           | 31.693       |
| Apple Bangkok              | 2022 | 2427853        | 2301700           | -5.196       |
| Apple Bangkok              | 2023 | 2301700        | 450951            | -80.408      |
| Apple Bangkok              | 2024 | 450951         | 273128            | -39.433      |
| Apple Barcelona            | 2021 | 1888830        | 1622698           | -14.09       |
| Apple Barcelona            | 2022 | 1622698        | 1740833           | 7.28         |
| Apple Barcelona            | 2023 | 1740833        | 1832208           | 5.249        |
| Apple Barcelona            | 2024 | 1832208        | 266734            | -85.442      |
| Apple Beijing              | 2021 | 1879950        | 2418725           | 28.659       |
| Apple Beijing              | 2022 | 2418725        | 2393955           | -1.024       |
| Apple Beijing              | 2023 | 2393955        | 435168            | -81.822      |
| Apple Beijing              | 2024 | 435168         | 250150            | -42.516      |
| Apple Birmingham           | 2024 | 423178         | 256344            | -39.424      |
| Apple Bluewater            | 2020 | 1883138        | 2077446           | 10.318       |
| Apple Bluewater            | 2021 | 2077446        | 3066010           | 47.586       |
| Apple Bluewater            | 2022 | 3066010        | 8782765           | 186.456      |
| Apple Bluewater            | 2023 | 8782765        | 2481314           | -71.748      |
| Apple Bluewater            | 2024 | 2481314        | 159437            | -93.574      |
| Apple Brisbane             | 2021 | 1919241        | 2642050           | 37.661       |
| Apple Brisbane             | 2022 | 2642050        | 3922789           | 48.475       |
| Apple Brisbane             | 2023 | 3922789        | 1662969           | -57.607      |
| Apple Brisbane             | 2024 | 1662969        | 251150            | -84.897      |
| Apple Buenos Aires         | 2022 | 2397762        | 2266156           | -5.489       |
| Apple Buenos Aires         | 2023 | 2266156        | 439957            | -80.586      |
| Apple Buenos Aires         | 2024 | 439957         | 250956            | -42.959      |
| Apple Caracas              | 2022 | 2328610        | 2356394           | 1.193        |
| Apple Caracas              | 2023 | 2356394        | 450145            | -80.897      |
| Apple Caracas              | 2024 | 450145         | 272531            | -39.457      |
| Apple CF Sherway Gardens   | 2020 | 1837609        | 2051005           | 11.613       |
| Apple CF Sherway Gardens   | 2021 | 2051005        | 3080633           | 50.201       |
| Apple CF Sherway Gardens   | 2022 | 3080633        | 7151285           | 132.137      |
| Apple CF Sherway Gardens   | 2023 | 7151285        | 1586043           | -77.822      |
| Apple CF Sherway Gardens   | 2024 | 1586043        | 178818            | -88.726      |
| Apple Champs-Ã‰lysÃ©es     | 2020 | 1816915        | 2027009           | 11.563       |
| Apple Champs-Ã‰lysÃ©es     | 2021 | 2027009        | 2962589           | 46.156       |
| Apple Champs-Ã‰lysÃ©es     | 2022 | 2962589        | 3508024           | 18.411       |
| Apple Champs-Ã‰lysÃ©es     | 2023 | 3508024        | 1503311           | -57.147      |
| Apple Champs-Ã‰lysÃ©es     | 2024 | 1503311        | 190412            | -87.334      |
| Apple Chengdu              | 2021 | 1919300        | 2527660           | 31.697       |
| Apple Chengdu              | 2022 | 2527660        | 2290186           | -9.395       |
| Apple Chengdu              | 2023 | 2290186        | 454954            | -80.135      |
| Apple Chengdu              | 2024 | 454954         | 254946            | -43.962      |
| Apple Chinook Centre       | 2020 | 1906045        | 2042055           | 7.136        |
| Apple Chinook Centre       | 2021 | 2042055        | 2977616           | 45.815       |
| Apple Chinook Centre       | 2022 | 2977616        | 6968563           | 134.032      |
| Apple Chinook Centre       | 2023 | 6968563        | 1609351           | -76.906      |
| Apple Chinook Centre       | 2024 | 1609351        | 208191            | -87.064      |
| Apple Covent Garden        | 2020 | 1855319        | 2051412           | 10.569       |
| Apple Covent Garden        | 2021 | 2051412        | 3019032           | 47.168       |
| Apple Covent Garden        | 2022 | 3019032        | 8796249           | 191.36       |
| Apple Covent Garden        | 2023 | 8796249        | 2515756           | -71.4        |
| Apple Covent Garden        | 2024 | 2515756        | 188408            | -92.511      |
| Apple Doha                 | 2022 | 2485293        | 2366895           | -4.764       |
| Apple Doha                 | 2023 | 2366895        | 405189            | -82.881      |
| Apple Doha                 | 2024 | 405189         | 256542            | -36.686      |
| Apple Dubai Mall           | 2021 | 1881434        | 2972841           | 58.009       |
| Apple Dubai Mall           | 2022 | 2972841        | 3530467           | 18.757       |
| Apple Dubai Mall           | 2023 | 3530467        | 1102879           | -68.761      |
| Apple Eastview Mall        | 2020 | 7787628        | 4506982           | -42.126      |
| Apple Eastview Mall        | 2021 | 4506982        | 3105538           | -31.095      |
| Apple Eastview Mall        | 2022 | 3105538        | 7172416           | 130.956      |
| Apple Eastview Mall        | 2023 | 7172416        | 1540332           | -78.524      |
| Apple Eastview Mall        | 2024 | 1540332        | 194408            | -87.379      |
| Apple Eaton Centre         | 2020 | 7759153        | 4390230           | -43.419      |
| Apple Eaton Centre         | 2021 | 4390230        | 3287229           | -25.124      |
| Apple Eaton Centre         | 2022 | 3287229        | 7116004           | 116.474      |
| Apple Eaton Centre         | 2023 | 7116004        | 1583458           | -77.748      |
| Apple Eaton Centre         | 2024 | 1583458        | 196203            | -87.609      |
| Apple Fifth Avenue         | 2020 | 7999505        | 16056857          | 100.723      |
| Apple Fifth Avenue         | 2021 | 16056857       | 3309992           | -79.386      |
| Apple Fifth Avenue         | 2022 | 3309992        | 7147136           | 115.926      |
| Apple Fifth Avenue         | 2023 | 7147136        | 1587039           | -77.795      |
| Apple Fifth Avenue         | 2024 | 1587039        | 161635            | -89.815      |
| Apple Ho Chi Minh City     | 2022 | 2415362        | 2306863           | -4.492       |
| Apple Ho Chi Minh City     | 2023 | 2306863        | 412789            | -82.106      |
| Apple Ho Chi Minh City     | 2024 | 412789         | 220382            | -46.611      |
| Apple Hong Kong            | 2021 | 1870551        | 2456885           | 31.346       |
| Apple Hong Kong            | 2022 | 2456885        | 2353050           | -4.226       |
| Apple Hong Kong            | 2023 | 2353050        | 433569            | -81.574      |
| Apple Hong Kong            | 2024 | 433569         | 247754            | -42.857      |
| Apple Istanbul             | 2021 | 1791436        | 2424412           | 35.333       |
| Apple Istanbul             | 2022 | 2424412        | 2373341           | -2.107       |
| Apple Istanbul             | 2023 | 2373341        | 15835733          | 567.234      |
| Apple Istanbul             | 2024 | 15835733       | 254149            | -98.395      |
| Apple Jakarta              | 2021 | 1865004        | 2608645           | 39.873       |
| Apple Jakarta              | 2022 | 2608645        | 2246840           | -13.869      |
| Apple Jakarta              | 2023 | 2246840        | 437560            | -80.526      |
| Apple Jakarta              | 2024 | 437560         | 230768            | -47.26       |
| Apple Kuala Lumpur         | 2021 | 1867248        | 2492404           | 33.48        |
| Apple Kuala Lumpur         | 2022 | 2492404        | 2434960           | -2.305       |
| Apple Kuala Lumpur         | 2023 | 2434960        | 448549            | -81.579      |
| Apple Kuala Lumpur         | 2024 | 448549         | 259344            | -42.182      |
| Apple KurfÃ¼rstendamm      | 2020 | 1936166        | 2014526           | 4.047        |
| Apple KurfÃ¼rstendamm      | 2021 | 2014526        | 3065504           | 52.17        |
| Apple KurfÃ¼rstendamm      | 2022 | 3065504        | 3567034           | 16.36        |
| Apple KurfÃ¼rstendamm      | 2023 | 3567034        | 1418070           | -60.245      |
| Apple KurfÃ¼rstendamm      | 2024 | 1418070        | 171425            | -87.911      |
| Apple La Paz               | 2022 | 2483917        | 2245284           | -9.607       |
| Apple La Paz               | 2023 | 2245284        | 422379            | -81.188      |
| Apple La Paz               | 2024 | 422379         | 276524            | -34.532      |
| Apple Lennox Town Center   | 2020 | 7813311        | 4492668           | -42.5        |
| Apple Lennox Town Center   | 2021 | 4492668        | 3096559           | -31.075      |
| Apple Lennox Town Center   | 2022 | 3096559        | 7177715           | 131.796      |
| Apple Lennox Town Center   | 2023 | 7177715        | 1630305           | -77.287      |
| Apple Lennox Town Center   | 2024 | 1630305        | 198201            | -87.843      |
| Apple Lima                 | 2022 | 2519083        | 2457473           | -2.446       |
| Apple Lima                 | 2023 | 2457473        | 475926            | -80.634      |
| Apple Lima                 | 2024 | 475926         | 262538            | -44.836      |
| Apple Lyon                 | 2020 | 1844232        | 1979475           | 7.333        |
| Apple Lyon                 | 2021 | 1979475        | 3018773           | 52.504       |
| Apple Lyon                 | 2022 | 3018773        | 3633022           | 20.348       |
| Apple Lyon                 | 2023 | 3633022        | 1492582           | -58.916      |
| Apple Lyon                 | 2024 | 1492582        | 168233            | -88.729      |
| Apple Madrid               | 2021 | 1872574        | 2358589           | 25.954       |
| Apple Madrid               | 2022 | 2358589        | 3746663           | 58.852       |
| Apple Madrid               | 2023 | 3746663        | 1719412           | -54.108      |
| Apple Madrid               | 2024 | 1719412        | 266133            | -84.522      |
| Apple Mall of America      | 2020 | 7707491        | 4285784           | -44.395      |
| Apple Mall of America      | 2021 | 4285784        | 3328784           | -22.33       |
| Apple Mall of America      | 2022 | 3328784        | 7165870           | 115.27       |
| Apple Mall of America      | 2023 | 7165870        | 1562599           | -78.194      |
| Apple Mall of America      | 2024 | 1562599        | 196605            | -87.418      |
| Apple Mall of the Emirates | 2021 | 1819669        | 1564407           | -14.028      |
| Apple Mall of the Emirates | 2022 | 1564407        | 851749            | -45.555      |
| Apple Mall of the Emirates | 2023 | 851749         | 307893            | -63.852      |
| Apple Manchester           | 2024 | 455548         | 260338            | -42.852      |
| Apple Manila               | 2022 | 2488500        | 2324253           | -6.6         |
| Apple Manila               | 2023 | 2324253        | 453145            | -80.504      |
| Apple Manila               | 2024 | 453145         | 268934            | -40.652      |
| Apple Marunouchi           | 2020 | 2004466        | 1951028           | -2.666       |
| Apple Marunouchi           | 2021 | 1951028        | 2825241           | 44.808       |
| Apple Marunouchi           | 2022 | 2825241        | 6943514           | 145.767      |
| Apple Marunouchi           | 2023 | 6943514        | 1579544           | -77.252      |
| Apple Marunouchi           | 2024 | 1579544        | 178618            | -88.692      |
| Apple Melbourne            | 2021 | 1969459        | 2468209           | 25.324       |
| Apple Melbourne            | 2022 | 2468209        | 3678971           | 49.054       |
| Apple Melbourne            | 2023 | 3678971        | 1712708           | -53.446      |
| Apple Melbourne            | 2024 | 1712708        | 225172            | -86.853      |
| Apple Mexico City          | 2021 | 1839020        | 2458520           | 33.686       |
| Apple Mexico City          | 2022 | 2458520        | 3770181           | 53.352       |
| Apple Mexico City          | 2023 | 3770181        | 1736705           | -53.936      |
| Apple Mexico City          | 2024 | 1736705        | 271131            | -84.388      |
| Apple Michigan Avenue      | 2020 | 7867594        | 16415530          | 108.647      |
| Apple Michigan Avenue      | 2021 | 16415530       | 3285077           | -79.988      |
| Apple Michigan Avenue      | 2022 | 3285077        | 7267663           | 121.233      |
| Apple Michigan Avenue      | 2023 | 7267663        | 1656217           | -77.211      |
| Apple Michigan Avenue      | 2024 | 1656217        | 186412            | -88.745      |
| Apple Milan                | 2020 | 1793696        | 2073885           | 15.621       |
| Apple Milan                | 2021 | 2073885        | 2951337           | 42.31        |
| Apple Milan                | 2022 | 2951337        | 3564439           | 20.774       |
| Apple Milan                | 2023 | 3564439        | 1484782           | -58.345      |
| Apple Milan                | 2024 | 1484782        | 194607            | -86.893      |
| Apple Montevideo           | 2022 | 2467321        | 2327318           | -5.674       |
| Apple Montevideo           | 2023 | 2327318        | 434968            | -81.31       |
| Apple Montevideo           | 2024 | 434968         | 263537            | -39.412      |
| Apple Mumbai               | 2021 | 1867936        | 2503068           | 34.002       |
| Apple Mumbai               | 2022 | 2503068        | 2434097           | -2.755       |
| Apple Mumbai               | 2023 | 2434097        | 16079249          | 560.584      |
| Apple Mumbai               | 2024 | 16079249       | 216181            | -98.656      |
| Apple Munich               | 2020 | 1889324        | 1976799           | 4.63         |
| Apple Munich               | 2021 | 1976799        | 3024655           | 53.008       |
| Apple Munich               | 2022 | 3024655        | 8737730           | 188.884      |
| Apple Munich               | 2023 | 8737730        | 2510950           | -71.263      |
| Apple Munich               | 2024 | 2510950        | 180820            | -92.799      |
| Apple New Delhi            | 2021 | 1934114        | 2393903           | 23.773       |
| Apple New Delhi            | 2022 | 2393903        | 2455916           | 2.59         |
| Apple New Delhi            | 2023 | 2455916        | 16135777          | 557.017      |
| Apple New Delhi            | 2024 | 16135777       | 282518            | -98.249      |
| Apple Newcastle            | 2024 | 412391         | 276924            | -32.849      |
| Apple Orchard Road         | 2020 | 1844980        | 1971568           | 6.861        |
| Apple Orchard Road         | 2021 | 1971568        | 2971015           | 50.693       |
| Apple Orchard Road         | 2022 | 2971015        | 8892638           | 199.313      |
| Apple Orchard Road         | 2023 | 8892638        | 2562428           | -71.185      |
| Apple Orchard Road         | 2024 | 2562428        | 171830            | -93.294      |
| Apple Pacific Centre       | 2020 | 7963478        | 4454218           | -44.067      |
| Apple Pacific Centre       | 2021 | 4454218        | 3109032           | -30.2        |
| Apple Pacific Centre       | 2022 | 3109032        | 7157187           | 130.206      |
| Apple Pacific Centre       | 2023 | 7157187        | 1664166           | -76.748      |
| Apple Pacific Centre       | 2024 | 1664166        | 187615            | -88.726      |
| Apple Regent Street        | 2020 | 1955207        | 2047942           | 4.743        |
| Apple Regent Street        | 2021 | 2047942        | 3045747           | 48.722       |
| Apple Regent Street        | 2022 | 3045747        | 8826923           | 189.811      |
| Apple Regent Street        | 2023 | 8826923        | 2486424           | -71.831      |
| Apple Regent Street        | 2024 | 2486424        | 181816            | -92.688      |
| Apple Rideau Centre        | 2020 | 1888917        | 1990404           | 5.373        |
| Apple Rideau Centre        | 2021 | 1990404        | 3099261           | 55.71        |
| Apple Rideau Centre        | 2022 | 3099261        | 7000383           | 125.873      |
| Apple Rideau Centre        | 2023 | 7000383        | 1646846           | -76.475      |
| Apple Rideau Centre        | 2024 | 1646846        | 172226            | -89.542      |
| Apple Rio de Janeiro       | 2021 | 1973696        | 2594928           | 31.476       |
| Apple Rio de Janeiro       | 2022 | 2594928        | 3696157           | 42.438       |
| Apple Rio de Janeiro       | 2023 | 3696157        | 1688600           | -54.315      |
| Apple Rio de Janeiro       | 2024 | 1688600        | 253943            | -84.961      |
| Apple Riyadh               | 2022 | 2349687        | 2286765           | -2.678       |
| Apple Riyadh               | 2023 | 2286765        | 411191            | -82.019      |
| Apple Riyadh               | 2024 | 411191         | 268129            | -34.792      |
| Apple San JosÃ©            | 2022 | 2367111        | 2469758           | 4.336        |
| Apple San JosÃ©            | 2023 | 2469758        | 423977            | -82.833      |
| Apple San JosÃ©            | 2024 | 423977         | 248952            | -41.282      |
| Apple Santiago             | 2021 | 1901932        | 2450036           | 28.818       |
| Apple Santiago             | 2022 | 2450036        | 2434170           | -0.648       |
| Apple Santiago             | 2023 | 2434170        | 416788            | -82.878      |
| Apple Santiago             | 2024 | 416788         | 259740            | -37.681      |
| Apple Santo Domingo        | 2024 | 451347         | 258944            | -42.629      |
| Apple SÃ£o Paulo           | 2021 | 1856896        | 2560796           | 37.907       |
| Apple SÃ£o Paulo           | 2022 | 2560796        | 3686096           | 43.943       |
| Apple SÃ£o Paulo           | 2023 | 3686096        | 1706267           | -53.711      |
| Apple SÃ£o Paulo           | 2024 | 1706267        | 239957            | -85.937      |
| Apple Scottsdale           | 2020 | 7861876        | 4552707           | -42.091      |
| Apple Scottsdale           | 2021 | 4552707        | 3170286           | -30.365      |
| Apple Scottsdale           | 2022 | 3170286        | 7044386           | 122.2        |
| Apple Scottsdale           | 2023 | 7044386        | 1646181           | -76.631      |
| Apple Scottsdale           | 2024 | 1646181        | 158444            | -90.375      |
| Apple Seoul                | 2021 | 1843305        | 2502079           | 35.739       |
| Apple Seoul                | 2022 | 2502079        | 2433547           | -2.739       |
| Apple Seoul                | 2023 | 2433547        | 16014758          | 558.083      |
| Apple Seoul                | 2024 | 16014758       | 275728            | -98.278      |
| Apple Shanghai             | 2021 | 1851126        | 2446399           | 32.157       |
| Apple Shanghai             | 2022 | 2446399        | 2399675           | -1.91        |
| Apple Shanghai             | 2023 | 2399675        | 445954            | -81.416      |
| Apple Shanghai             | 2024 | 445954         | 278518            | -37.546      |
| Apple Sheffield            | 2024 | 392009         | 257544            | -34.302      |
| Apple Shibuya              | 2020 | 1831094        | 2095992           | 14.467       |
| Apple Shibuya              | 2021 | 2095992        | 3015576           | 43.873       |
| Apple Shibuya              | 2022 | 3015576        | 7111654           | 135.831      |
| Apple Shibuya              | 2023 | 7111654        | 1631774           | -77.055      |
| Apple Shibuya              | 2024 | 1631774        | 188210            | -88.466      |
| Apple South Coast Plaza    | 2020 | 7675300        | 18774726          | 144.612      |
| Apple South Coast Plaza    | 2021 | 18774726       | 3428726           | -81.738      |
| Apple South Coast Plaza    | 2022 | 3428726        | 6939638           | 102.397      |
| Apple South Coast Plaza    | 2023 | 6939638        | 1528512           | -77.974      |
| Apple South Coast Plaza    | 2024 | 1528512        | 177020            | -88.419      |
| Apple Square One           | 2020 | 7814160        | 4407295           | -43.599      |
| Apple Square One           | 2021 | 4407295        | 3098452           | -29.697      |
| Apple Square One           | 2022 | 3098452        | 7138057           | 130.375      |
| Apple Square One           | 2023 | 7138057        | 1658920           | -76.76       |
| Apple Square One           | 2024 | 1658920        | 191208            | -88.474      |
| Apple Sydney               | 2021 | 1870355        | 2499240           | 33.624       |
| Apple Sydney               | 2022 | 2499240        | 3708288           | 48.377       |
| Apple Sydney               | 2023 | 3708288        | 1707957           | -53.942      |
| Apple Sydney               | 2024 | 1707957        | 294910            | -82.733      |
| Apple Taipei               | 2021 | 1904118        | 2573863           | 35.174       |
| Apple Taipei               | 2022 | 2573863        | 2258025           | -12.271      |
| Apple Taipei               | 2023 | 2258025        | 426171            | -81.126      |
| Apple Taipei               | 2024 | 426171         | 270528            | -36.521      |
| Apple The Grove            | 2020 | 7976698        | 16321288          | 104.612      |
| Apple The Grove            | 2021 | 16321288       | 3351102           | -79.468      |
| Apple The Grove            | 2022 | 3351102        | 7115266           | 112.326      |
| Apple The Grove            | 2023 | 7115266        | 1555903           | -78.133      |
| Apple The Grove            | 2024 | 1555903        | 202194            | -87.005      |
| Apple Tysons Corner        | 2020 | 7592160        | 4469531           | -41.13       |
| Apple Tysons Corner        | 2021 | 4469531        | 3121483           | -30.161      |
| Apple Tysons Corner        | 2022 | 3121483        | 7138609           | 128.693      |
| Apple Tysons Corner        | 2023 | 7138609        | 1567890           | -78.036      |
| Apple Tysons Corner        | 2024 | 1567890        | 159439            | -89.831      |
| Apple Union Square         | 2020 | 7739092        | 16334567          | 111.066      |
| Apple Union Square         | 2021 | 16334567       | 3334158           | -79.588      |
| Apple Union Square         | 2022 | 3334158        | 7091889           | 112.704      |
| Apple Union Square         | 2023 | 7091889        | 1594081           | -77.522      |
| Apple Union Square         | 2024 | 1594081        | 200397            | -87.429      |
| Apple West Edmonton Mall   | 2020 | 1819405        | 1974559           | 8.528        |
| Apple West Edmonton Mall   | 2021 | 1974559        | 2983962           | 51.12        |
| Apple West Edmonton Mall   | 2022 | 2983962        | 6995602           | 134.44       |
| Apple West Edmonton Mall   | 2023 | 6995602        | 1605885           | -77.044      |
| Apple West Edmonton Mall   | 2024 | 1605885        | 164834            | -89.736      |
| Apple Yorkdale             | 2020 | 7976055        | 4324581           | -45.78       |
| Apple Yorkdale             | 2021 | 4324581        | 3202151           | -25.955      |
| Apple Yorkdale             | 2022 | 3202151        | 7377481           | 130.391      |
| Apple Yorkdale             | 2023 | 7377481        | 1620632           | -78.033      |
| Apple Yorkdale             | 2024 | 1620632        | 202399            | -87.511      |
| Apple Zurich               | 2021 | 1835007        | 2675477           | 45.802       |
| Apple Zurich               | 2022 | 2675477        | 3722353           | 39.129       |
| Apple Zurich               | 2023 | 3722353        | 1746931           | -53.069      |
| Apple Zurich               | 2024 | 1746931        | 259142            | -85.166      |


</details><br>

18. Calculate the correlation between product price and warranty claims for products sold in the last five years, segmented by price range.
```sql
SELECT 
	
	CASE
		WHEN p.price < 500 THEN 'Less Expenses Product'
		WHEN p.price BETWEEN 500 AND 1000 THEN 'Mid Range Product'
		ELSE 'Expensive Product'
	END as price_segment,
	COUNT(w.claim_id) as total_Claim
FROM warranty as w
LEFT JOIN
sales as s
ON w.sale_id = s.sale_id
JOIN 
products as p
ON p.product_id = s.product_id
WHERE claim_date >= CURRENT_DATE - INTERVAL '5 year'
GROUP BY 1
```
<details>
  <summary>Data Output</summary>

| price_segment         | total_claim |
|-----------------------|-------------|
| Expensive Product     | 4463        |
| Less Expenses Product | 14417       |
| Mid Range Product     | 11943       |

</details><br>

19. Identify the store with the highest percentage of "Paid Repaired" claims relative to total claims filed.
```sql
WITH paid_repair
AS
(SELECT 
	s.store_id,
	COUNT(w.claim_id) as paid_repaired
FROM sales as s
RIGHT JOIN warranty as w
ON w.sale_id = s.sale_id
WHERE w.repair_status = 'Paid Repaired'
GROUP BY 1
),

total_repaired
AS
(SELECT 
	s.store_id,
	COUNT(w.claim_id) as total_repaired
FROM sales as s
RIGHT JOIN warranty as w
ON w.sale_id = s.sale_id
GROUP BY 1)

SELECT 
	tr.store_id,
	st.store_name,
	pr.paid_repaired,
	tr.total_repaired,
	ROUND(pr.paid_repaired::numeric/
			tr.total_repaired::numeric * 100
		,2) as percentage_paid_repaired
FROM paid_repair as pr
JOIN 
total_repaired tr
ON pr.store_id = tr.store_id
JOIN stores as st
ON tr.store_id = st.store_id
```
<details>
  <summary>Data Output</summary>

| store_id | store_name                 | paid_repaired | total_repaired | percentage_paid_repaired |
|----------|----------------------------|---------------|----------------|--------------------------|
| ST-22    | Apple Regent Street        | 377           | 1214           | 31.05                    |
| ST-23    | Apple Covent Garden        | 86            | 267            | 32.21                    |
| ST-24    | Apple Bluewater            | 86            | 259            | 33.20                    |
| ST-25    | Apple Munich               | 147           | 481            | 30.56                    |
| ST-26    | Apple Kurfürstendamm       | 60            | 160            | 37.50                    |
| ST-27    | Apple Amsterdam            | 52            | 163            | 31.90                    |
| ST-28    | Apple Champs-Élysées       | 53            | 151            | 35.10                    |
| ST-29    | Apple Lyon                 | 50            | 184            | 27.17                    |
| ST-30    | Apple Milan                | 417           | 1691           | 24.66                    |
| ST-31    | Apple Dubai Mall           | 1427          | 5838           | 24.44                    |
| ST-32    | Apple Mall of the Emirates | 771           | 5978           | 12.90                    |

</details><br>

20. Write a query to calculate the monthly running total of sales for each store over the past four years and compare trends during this period.
```sql
WITH monthly_sales
AS
(SELECT 
	store_id,
	EXTRACT(YEAR FROM sale_date) as year,
	EXTRACT(MONTH FROM sale_date) as month,
	SUM(p.price * s.quantity) as total_revenue
FROM sales as s
JOIN 
products as p
ON s.product_id = p.product_id
GROUP BY 1, 2, 3
ORDER BY 1, 2,3
)
SELECT 
	store_id,
	month,
	year,
	total_revenue,
	SUM(total_revenue) OVER(PARTITION BY store_id ORDER BY year, month) as running_total
FROM monthly_sales
```
<details>
  <summary>Data Output</summary>

[Data Output Excel File](https://github.com/akashdeep-portfolio/Apple_Retails_SalesAnalysis_1MillionRows-SQL_Advance/blob/main/Q20_DataOutput.xlsx)

</details><br>

### Bonus Question

- Analyze product sales trends over time, segmented into key periods: from launch to 6 months, 6-12 months, 12-18 months, and beyond 18 months.
```sql
SELECT 
	p.product_name,
	CASE 
		WHEN s.sale_date BETWEEN p.launch_date AND p.launch_date + INTERVAL '6 month' THEN '0-6 month'
		WHEN s.sale_date BETWEEN  p.launch_date + INTERVAL '6 month'  AND p.launch_date + INTERVAL '12 month' THEN '6-12' 
		WHEN s.sale_date BETWEEN  p.launch_date + INTERVAL '12 month'  AND p.launch_date + INTERVAL '18 month' THEN '6-12'
		ELSE '18+'
	END as plc,
	SUM(s.quantity) as total_qty_sale
	
FROM sales as s
JOIN products as p
ON s.product_id = p.product_id
GROUP BY 1, 2
ORDER BY 1, 3 DESC 
```
<details>
  <summary>Data Output</summary>

| product_name                  | plc       | total_qty_sale |
|-------------------------------|-----------|----------------|
| AirPods                       | 18+       | 5287           |
| AirPods (2nd Gen)             | 18+       | 3375           |
| AirPods (2nd Gen)             | 0-6 month | 2294           |
| AirPods (2nd Gen)             | 6-12      | 1354           |
| AirPods (3rd Gen)             | 0-6 month | 6099           |
| AirPods (3rd Gen)             | 6-12      | 3019           |
| AirPods (3rd Gen)             | 18+       | 1615           |
| AirPods Max                   | 0-6 month | 5482           |
| AirPods Max                   | 18+       | 2627           |
| AirPods Max                   | 6-12      | 1648           |
| AirPods Pro (2nd Gen)         | 0-6 month | 20823          |
| AirPods Pro (2nd Gen)         | 6-12      | 5955           |
| AirTag                        | 0-6 month | 82549          |
| AirTag                        | 6-12      | 45140          |
| AirTag                        | 18+       | 2015           |
| Apple Fitness+                | 0-6 month | 5376           |
| Apple Fitness+                | 18+       | 2671           |
| Apple Fitness+                | 6-12      | 1610           |
| Apple One                     | 18+       | 3015           |
| Apple One                     | 0-6 month | 2097           |
| Apple One                     | 6-12      | 417            |
| Apple TV 4K                   | 18+       | 6562           |
| Apple TV 4K                   | 0-6 month | 4932           |
| Apple TV 4K                   | 6-12      | 4555           |
| Apple Watch SE (2nd Gen)      | 0-6 month | 10474          |
| Apple Watch SE (2nd Gen)      | 6-12      | 3180           |
| Apple Watch Series 3          | 18+       | 4692           |
| Apple Watch Series 3          | 6-12      | 614            |
| Apple Watch Series 4          | 18+       | 15951          |
| Apple Watch Series 4          | 6-12      | 11579          |
| Apple Watch Series 4          | 0-6 month | 646            |
| Apple Watch Series 5          | 18+       | 3385           |
| Apple Watch Series 5          | 0-6 month | 3184           |
| Apple Watch Series 5          | 6-12      | 483            |
| Apple Watch Series 7          | 0-6 month | 5997           |
| Apple Watch Series 7          | 6-12      | 3021           |
| Apple Watch Series 7          | 18+       | 1621           |
| Apple Watch Series 8          | 0-6 month | 10425          |
| Apple Watch Series 8          | 6-12      | 3234           |
| HomePod mini                  | 18+       | 2796           |
| HomePod mini                  | 0-6 month | 1795           |
| HomePod mini                  | 6-12      | 771            |
| iMac (24-inch, M1)            | 0-6 month | 4851           |
| iMac (24-inch, M1)            | 6-12      | 3947           |
| iMac (24-inch, M1)            | 18+       | 2086           |
| iPad (10th Gen)               | 0-6 month | 20203          |
| iPad (10th Gen)               | 6-12      | 6460           |
| iPad (6th Gen)                | 18+       | 26454          |
| iPad (6th Gen)                | 6-12      | 2477           |
| iPad (7th Gen)                | 18+       | 3380           |
| iPad (7th Gen)                | 0-6 month | 3205           |
| iPad (7th Gen)                | 6-12      | 410            |
| iPad (9th Gen)                | 0-6 month | 5890           |
| iPad (9th Gen)                | 6-12      | 3108           |
| iPad (9th Gen)                | 18+       | 1647           |
| iPad Air (4th Gen)            | 18+       | 3105           |
| iPad Air (4th Gen)            | 0-6 month | 2066           |
| iPad Air (4th Gen)            | 6-12      | 283            |
| iPad Air (5th Gen)            | 0-6 month | 7668           |
| iPad Air (5th Gen)            | 6-12      | 4395           |
| iPad Air (5th Gen)            | 18+       | 1652           |
| iPad Pro (10.5-inch)          | 18+       | 4859           |
| iPad Pro (12.9-inch, 2nd Gen) | 18+       | 5189           |
| iPad Pro (M1, 11-inch)        | 0-6 month | 4924           |
| iPad Pro (M1, 11-inch)        | 6-12      | 3868           |
| iPad Pro (M1, 11-inch)        | 18+       | 2056           |
| iPad Pro (M1, 12.9-inch)      | 0-6 month | 4939           |
| iPad Pro (M1, 12.9-inch)      | 6-12      | 3791           |
| iPad Pro (M1, 12.9-inch)      | 18+       | 2099           |
| iPad Pro (M2, 11-inch)        | 0-6 month | 20340          |
| iPad Pro (M2, 11-inch)        | 6-12      | 6393           |
| iPad Pro (M2, 12.9-inch)      | 0-6 month | 10206          |
| iPad Pro (M2, 12.9-inch)      | 6-12      | 3414           |
| iPhone 11                     | 0-6 month | 3349           |
| iPhone 11                     | 18+       | 3229           |
| iPhone 11                     | 6-12      | 445            |
| iPhone 11 Pro                 | 18+       | 3325           |
| iPhone 11 Pro                 | 0-6 month | 3260           |
| iPhone 11 Pro                 | 6-12      | 421            |
| iPhone 11 Pro Max             | 18+       | 3273           |
| iPhone 11 Pro Max             | 0-6 month | 3226           |
| iPhone 11 Pro Max             | 6-12      | 411            |
| iPhone 12                     | 0-6 month | 7914           |
| iPhone 12                     | 18+       | 2991           |
| iPhone 12                     | 6-12      | 386            |
| iPhone 12 Mini                | 18+       | 2846           |
| iPhone 12 Mini                | 0-6 month | 1854           |
| iPhone 12 Mini                | 6-12      | 787            |
| iPhone 12 Pro                 | 0-6 month | 7886           |
| iPhone 12 Pro                 | 18+       | 2985           |
| iPhone 12 Pro                 | 6-12      | 385            |
| iPhone 12 Pro Max             | 0-6 month | 5869           |
| iPhone 12 Pro Max             | 18+       | 2780           |
| iPhone 12 Pro Max             | 6-12      | 998            |
| iPhone 13                     | 0-6 month | 5906           |
| iPhone 13                     | 6-12      | 3040           |
| iPhone 13                     | 18+       | 1674           |
| iPhone 13 Mini                | 0-6 month | 2611           |
| iPhone 13 Mini                | 6-12      | 2106           |
| iPhone 13 Mini                | 18+       | 1664           |
| iPhone 13 Pro                 | 0-6 month | 6171           |
| iPhone 13 Pro                 | 6-12      | 3098           |
| iPhone 13 Pro                 | 18+       | 1668           |
| iPhone 13 Pro Max             | 0-6 month | 5993           |
| iPhone 13 Pro Max             | 6-12      | 3075           |
| iPhone 13 Pro Max             | 18+       | 1669           |
| iPhone 14                     | 0-6 month | 32936          |
| iPhone 14                     | 6-12      | 8229           |
| iPhone 14 Pro                 | 0-6 month | 32780          |
| iPhone 14 Pro                 | 6-12      | 8101           |
| iPhone 15                     | 0-6 month | 18490          |
| iPhone 15                     | 6-12      | 3167           |
| iPhone 15 Pro                 | 0-6 month | 18772          |
| iPhone 15 Pro                 | 6-12      | 3186           |
| iPhone 15 Pro Max             | 0-6 month | 18605          |
| iPhone 15 Pro Max             | 6-12      | 3179           |
| iPhone 8                      | 18+       | 4452           |
| iPhone 8                      | 6-12      | 726            |
| iPhone 8 Plus                 | 18+       | 4501           |
| iPhone 8 Plus                 | 6-12      | 633            |
| iPhone SE (3rd Gen)           | 0-6 month | 7564           |
| iPhone SE (3rd Gen)           | 6-12      | 4391           |
| iPhone SE (3rd Gen)           | 18+       | 1672           |
| iPhone X                      | 18+       | 61561          |
| iPhone X                      | 6-12      | 17046          |
| iPhone XR                     | 6-12      | 42194          |
| iPhone XR                     | 18+       | 15906          |
| iPhone XR                     | 0-6 month | 9827           |
| iPhone XS                     | 18+       | 16111          |
| iPhone XS                     | 6-12      | 12328          |
| iPhone XS                     | 0-6 month | 658            |
| iPhone XS Max                 | 18+       | 19210          |
| iPhone XS Max                 | 6-12      | 12063          |
| iPhone XS Max                 | 0-6 month | 584            |
| Mac mini (2018)               | 6-12      | 30154          |
| Mac mini (2018)               | 18+       | 30114          |
| Mac mini (2018)               | 0-6 month | 1011           |
| Mac mini (M1)                 | 18+       | 2809           |
| Mac mini (M1)                 | 0-6 month | 1865           |
| Mac mini (M1)                 | 6-12      | 713            |
| Mac Studio                    | 0-6 month | 7818           |
| Mac Studio                    | 6-12      | 4399           |
| Mac Studio                    | 18+       | 1679           |
| MacBook (2017)                | 18+       | 5209           |
| MacBook Air (M1)              | 18+       | 2869           |
| MacBook Air (M1)              | 0-6 month | 1917           |
| MacBook Air (M1)              | 6-12      | 788            |
| MacBook Air (Retina, 2018)    | 18+       | 4074           |
| MacBook Air (Retina, 2018)    | 6-12      | 3485           |
| MacBook Air (Retina, 2018)    | 0-6 month | 950            |
| MacBook Pro (13-inch, 2017)   | 18+       | 5410           |
| MacBook Pro (M1 Max, 16-inch) | 0-6 month | 41844          |
| MacBook Pro (M1 Max, 16-inch) | 6-12      | 14630          |
| MacBook Pro (M1 Max, 16-inch) | 18+       | 1742           |
| MacBook Pro (M1 Pro, 14-inch) | 0-6 month | 41710          |
| MacBook Pro (M1 Pro, 14-inch) | 6-12      | 14752          |
| MacBook Pro (M1 Pro, 14-inch) | 18+       | 1709           |
| MacBook Pro (M1, 13-inch)     | 18+       | 2902           |
| MacBook Pro (M1, 13-inch)     | 0-6 month | 1756           |
| MacBook Pro (M1, 13-inch)     | 6-12      | 778            |
| MacBook Pro (Touch Bar)       | 18+       | 5119           |

</details><br>

## Project Focus

This project primarily focuses on developing and showcasing the following SQL skills:

- **Complex Joins and Aggregations**: Demonstrating the ability to perform complex SQL joins and aggregate data meaningfully.
- **Window Functions**: Using advanced window functions for running totals, growth analysis, and time-based queries.
- **Data Segmentation**: Analyzing data across different time frames to gain insights into product performance.
- **Correlation Analysis**: Applying SQL functions to determine relationships between variables, such as product price and warranty claims.
- **Real-World Problem Solving**: Answering business-related questions that reflect real-world scenarios faced by data analysts.

## Conclusion

This project helped develop advanced SQL skills, including query optimization, window functions, and performance tuning for handling large datasets. It provided hands-on experience in extracting business insights, analyzing sales trends, warranty claims, and store performance. Working on this dataset improved the ability to solve real-world data challenges, enhance data-driven decision-making, and apply SQL analytics effectively. This will also help demonstrate expertise in SQL to potential employers, making it a strong addition to a professional portfolio.

---
