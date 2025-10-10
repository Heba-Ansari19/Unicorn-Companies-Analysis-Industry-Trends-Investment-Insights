# Analyzing-Unicorn-Companies
This project analyzes unicorn companies to help an investment firm identify the **top-performing industries from 2019–2021**. Using SQL (PostgreSQL), I explored industry growth, average valuations, and the emergence rate of high-value startups, providing insights that guide portfolio structuring and strategic investment decisions.
<br>
<br>
## PROJECT OVERVIEW

Unicorn companies are private startups valued at over $1 billion, and tracking their growth reveals where the next wave of innovation and opportunity lies. In this project, we supported an **investment firm by analyzing unicorn data across industries and years**. The firm wanted to understand **which industries are producing the most unicorns and how their valuations have evolved between 2019 and 2021.**

<img width="1000" height="931" alt="image" src="https://github.com/user-attachments/assets/521d3598-ef3c-4ec7-bb4b-f938911b52bd" />

By working with a structured unicorn database, my analysis focused on three critical areas: identifying the most promising industries, understanding how many new unicorns each industry produced during the selected years, and evaluating their average valuations in billions of dollars. These insights provide competitive intelligence for the firm, helping it recognize where to direct its future investment portfolio.
<br>
<br>
## OBJECTIVE / KEY QUESTIONS

* Which three industries produced the highest number of unicorns in 2019, 2020, and 2021 combined?
* How many unicorns did these industries generate each year?
* What was the average valuation (in billions, rounded to two decimals) of unicorns in these industries per year?
<br>

## DATASET

* Source: Unicorn Companies Database
* Tables:
    * `dates` – records when each company became a unicorn and its founding year.
      | Column        | Description                                 |
      | ------------- | ------------------------------------------- |
      | company\_id   | A unique ID for the company.                |
      | date\_joined  | The date that the company became a unicorn. |
      | year\_founded | The year that the company was founded.      |

    * `funding` – contains company valuations and total funding raised.
      | Column            | Description                                 |
      | ----------------- | ------------------------------------------- |
      | company\_id       | A unique ID for the company.                |
      | valuation         | Company value in US dollars.                |
      | funding           | The amount of funding raised in US dollars. |
      | select\_investors | A list of key investors in the company.     |

    * `industries` – shows the industry each company operates in.
      | Column      | Description                                |
      | ----------- | ------------------------------------------ |
      | company\_id | A unique ID for the company.               |
      | industry    | The industry that the company operates in. |

    * `companies` – provides details on the company name, location (city, country, continent).
      | Column      | Description                                       |
      | ----------- | ------------------------------------------------- |
      | company\_id | A unique ID for the company.                      |
      | company     | The name of the company.                          |
      | city        | The city where the company is headquartered.      |
      | country     | The country where the company is headquartered.   |
      | continent   | The continent where the company is headquartered. |
<br>

## TOOLS AND SKILLS USED

* [SQL (PostgreSQL)](https://www.postgresql.org/download/)
* Concepts: CTEs (Common Table Expressions), `JOIN`, Aggregations, Grouping, Filtering with `WHERE` and `EXTRACT`, Ordering, Subqueries.
* Techniques Used: Data exploration, industry benchmarking, trend analysis, valuation averaging, filtering by year, and summarizing performance.
<br>

## ANALYSIS & APPROACH

### 1. Data Exploration: Understanding the Tables
```sql

-- Overview of the table dates

SELECT * 
FROM dates
LIMIT 5;
```

<img width="800" alt="image" src="https://github.com/user-attachments/assets/7195fdcc-6ee2-4550-85b8-e83a1c64d8f7" />

```sql

-- Overview of the table funding

SELECT *
FROM funding
LIMIT 5;
```

<img width="800" alt="image" src="https://github.com/user-attachments/assets/272a6535-060f-48a1-96a7-fd96432e14a9" />

```sql

-- Overview of the table industries

SELECT *
FROM industries
LIMIT 5;
```

<img width="800" alt="image" src="https://github.com/user-attachments/assets/dddb462a-a2a4-423a-bb83-f62509750b83" />

```sql

-- Overview of the table companies

SELECT *
FROM companies
LIMIT 5;
```

<img width="800" alt="image" src="https://github.com/user-attachments/assets/0df32bbd-6ed1-4bb5-a198-da1d1a40547b" />

Before starting the main analysis, I explored the tables to **understand their structure and the kind of information they contained**. Checking a few rows from each table ensured the data was clean, consistent, and ready for aggregation and joins. Exploring the first few rows allowed me to **confirm that each table contained the expected information and that the data types were compatible for joins and calculations**. For example, I verified that `date_joined` could be extracted by year, valuation was numeric, and `company_id` was consistent across tables for proper joins.


### 2. Checking Columns and Data Types
```sql

SELECT table_name, column_name, data_type
FROM information_schema.columns
WHERE table_name IN ('dates', 'funding', 'industries', 'companies')
ORDER BY table_name, ordinal_position;
```

Using the `information_schema.columns` query, I **confirmed the column names, data types, and table structures.** This query lists all columns and their data types for the four main tables. **Knowing the data types was critical** — for example, confirming that valuation was numeric allowed me to calculate averages and convert to billions without errors. Checking the `date_joined` column type ensured I could **extract the year correctly for time-based analysis**. Additionally, seeing all columns in one place helped me decide which columns to include in joins, CTEs, and aggregations, streamlining the workflow and ensuring every query aligned with the table structure.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/26801d77-05e5-4b7a-88be-aad0d196f320" />


### 3. Identifying Top-Performing Industries (CTE1)
```sql

SELECT  
        i.industry,
        COUNT(*) AS number_of_companies
FROM industries AS i
INNER JOIN dates AS d
	USING(company_id)
WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
GROUP BY i.industry
ORDER BY number_of_companies DESC
LIMIT 3;
```

This step helped me find the **industries with the most unicorns created between 2019 and 2021**. I tested this CTE first to make sure the counts were correct, and it was identifying the top industries properly. By grouping and counting companies by industry, I focused only on the **strongest performers**. This was important because it let me ignore industries with very few unicorns and concentrate on the ones that really drive growth. Testing the CTE also helped confirm that the data matched correctly across tables.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/53068580-166a-4e82-adac-5a09939a4508" />


### 4. Calculating Yearly Valuations and Unicorn Counts (CTE2)
```sql

SELECT 
        i.industry,
        EXTRACT(YEAR FROM d.date_joined) AS year,
        COUNT(i.company_id) AS num_unicorns,
        ROUND(AVG(f.valuation), 2) AS avg_valuation
FROM industries AS i
INNER JOIN dates AS d
	USING(company_id)
INNER JOIN funding AS f
	USING(company_id)
WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
GROUP BY i.industry, year
ORDER BY year ASC;
```

Next, I created the second CTE to look at these **top industries year by year**, showing not just total unicorns but also how their numbers and `valuations` changed each year. I tested this CTE separately to ensure the counts and average valuations made sense. Calculating averages helped me understand financial health, while counting unicorns showed growth trends. This step was key because it let me see **which industries were growing faster or slower over time.**

<img width="800" alt="image" src="https://github.com/user-attachments/assets/6b2d9a86-ba42-4587-b123-0b8bd0f644a6" />


### 5. Final Combined Query (CTEs Integration)
```sql

--- CTE1 top_performing_industries

WITH top_performing_industries AS 
(
    SELECT  
        i.industry,
        COUNT(*) AS number_of_companies
    FROM industries AS i
    INNER JOIN dates AS d
    	USING(company_id)
    WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
    GROUP BY i.industry
    ORDER BY number_of_companies DESC
	LIMIT 3
),


--- CTE2 top_valuation

top_valuation AS 
(
    SELECT 
        i.industry,
        EXTRACT(YEAR FROM d.date_joined) AS year,
        COUNT(i.company_id) AS num_unicorns,
        ROUND(AVG(f.valuation), 2) AS avg_valuation
    FROM industries AS i
    INNER JOIN dates AS d
   		USING(company_id)
    INNER JOIN funding AS f
    	USING(company_id)
    WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
    GROUP BY i.industry, year
    ORDER BY year ASC
)



--- Final Query

SELECT
    industry,
    year,
    num_unicorns,
    ROUND(AVG(avg_valuation) / 1000000000, 2) AS average_valuation_billions
FROM top_valuation AS tv
INNER JOIN top_performing_industries AS tpi
	USING(industry)
WHERE year IN (2019, 2020, 2021)
  AND industry IN (SELECT industry FROM top_performing_industries)
GROUP BY industry, year, num_unicorns
ORDER BY year DESC, num_unicorns DESC;
```

Finally, I combined both CTEs in a single query, keeping only the **top industries and showing their yearly unicorn counts and average valuations**. I converted valuations into billions to make them easier to read. This provided both scale (`number_of_unicorns`) and value (`average_valuation`) — the two main factors investors care about. Testing the final query confirmed that all joins worked correctly and the numbers were reliable.

By testing each CTE and combining them carefully, I was able to get trustworthy insights about the top industries, how they grew over time, and their financial impact, which helps make smarter investment decisions.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/0fc2870b-cb34-435f-97f9-96530bf7d71e" />

<br>
<br>


## INSIGHTS AND FINDINGS

* **Fintech** led in overall unicorn creation, showing **rapid growth**, especially in **2021**.
* **Internet Software & Services** consistently produced a **large number of unicorns year after year**.
* **E-Commerce & Direct-to-Consumer** had **fewer unicorns compared to the other two**, but maintained **strong average valuations**.
* Valuations were highest in 2019 across industries, showing a possible cooling-off in later years despite higher unicorn counts.
* The **rise in 2021 unicorn counts** reflects global investor enthusiasm and easier access to funding.
<br>

## RECOMMENDATIONS

* Investment firms should **prioritize Fintech** as it shows the **strongest momentum and sheer scale of unicorn creation**.
* Internet Software & Services remains a **safe bet with consistency and proven year-over-year performance**.
* E-Commerce unicorns, though fewer, display **healthy valuations and could offer selective high-return opportunities**.
* **Continuous tracking of valuations is necessary** since higher unicorn counts don’t always mean higher valuations — **market dynamics shift fast**.
* Diversifying across these three industries **may provide the firm both stability (software) and aggressive growth** (fintech/e-commerce).
<br>

## FINAL NOTES

This project demonstrated how I used SQL to uncover meaningful business insights from structured data. By narrowing the analysis to the top three industries, I provided the investment firm with a clear, evidence-backed picture of where the unicorn ecosystem is thriving. These findings help guide **smarter portfolio strategies, ensuring decisions are based on industry performance trends rather than speculation**. Future extensions could include geographical comparisons or investor-specific performance to provide an even more targeted investment strategy.
<br>
<br>
## REFERENCES

* [DataCamp](https://app.datacamp.com/)
* [What Is the Average Stock Market Return?](https://www.nerdwallet.com/article/investing/average-stock-market-return)
<br>
<br>
