# Unicorn Companies Analysis: Industry Trends & Investment Insights
This project analyzes unicorn companies to help an investment firm identify the **top-performing industries from 2019–2021**. Using SQL (PostgreSQL), I explored industry growth, average valuations, and the emergence rate of high-value startups, providing insights that guide portfolio structuring and strategic investment decisions.
<br>
<br>
## PROJECT OVERVIEW

The average annual return from stock market investments is around 10%, but investment firms aim to outperform this benchmark by identifying faster-growing and higher-value opportunities.

In this project, I support an investment firm by analyzing unicorn companies—privately held startups valued at over $1 billion. The analysis focuses on understanding which industries are producing the most unicorns, how valuations differ across industries, how quickly companies achieve unicorn status, and whether these valuations are stable or concentrated among a few firms.

By answering these questions, the project provides insights into emerging industry trends, capital efficiency, and investment risk. These insights can help the firm decide where to allocate capital and how to structure its portfolio for long-term growth.

<img width="1000" height="931" alt="image" src="https://github.com/user-attachments/assets/521d3598-ef3c-4ec7-bb4b-f938911b52bd" />

<br>

## OBJECTIVE / KEY QUESTIONS

* Which industries produce the highest number of unicorn companies?
* How do unicorn valuations differ across industries and over time?
* How long does it take companies to become unicorns across industries?
* Which industries generate higher valuations relative to funding raised?
* Which industries show the strongest year-over-year growth in unicorn creation?
* Is unicorn valuation concentrated among a few companies within industries?
* Are unicorn valuations becoming more or less stable over time?
  
<br>

## DATASET

* Source: Unicorn companies database (provided for analysis)
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

* [SQL (PostgreSQL)](https://www.postgresql.org/download/) – Data querying, aggregation, and analysis

<br>

## ANALYSIS & APPROACH

### 1. Data Cleaning
The dataset was first validated to ensure reliability. I checked for missing values, incorrect data types, and unrealistic ranges in dates, funding, and valuations. No major data quality issues were identified, allowing the analysis to proceed without forced cleaning. This step ensured that all insights were based on complete and consistent data.


### 2. Exploratory Analysis
The initial exploration focused on understanding the overall structure of the data, including time ranges and the distribution of valuations and funding. Early patterns such as dominant industries and valuation ranges helped guide deeper analysis into growth trends, efficiency, and risk.

### 3. Advanced Querying
Advanced SQL techniques such as Common Table Expressions (CTEs), window functions, percentile calculations, and year-over-year comparisons were used. These methods allowed meaningful industry comparisons, trend analysis over time, and identification of concentration and volatility patterns that simple aggregations could not reveal.

### 4. Visualization
While the core analysis was done using SQL, visual summaries (when applied) help highlight trends such as valuation growth, volatility, and industry dominance. Visuals make complex numerical patterns easier to understand for non-technical stakeholders.

<br>

## INSIGHTS AND FINDINGS

* **Fintech**, **Internet Software & Services**, and **E-commerce** are the **top unicorn-producing industries**, together accounting for a very large share of total unicorn companies. This shows strong and consistent activity in these sectors.
* Unicorn creation **increased sharply in 2021**, especially in **Fintech (138 companies)**, **Internet Software & Services (119)**, and **E-commerce (47)**, indicating a surge in high-growth startups during this period.
* Valuation growth **declined over time for top industries**. While average valuations were **highest in 2019, they dropped noticeably by 2021**, even as the number of unicorns increased. This suggests **more companies reached unicorn status, but at lower average valuations**.
* Time to unicorn status varies by industry, but most companies take between **5 to 8 years to become unicorns**. **Auto & Transportation** and **Artificial Intelligence** reach unicorn status **faster**, while **Health**, **Data Analytics**, and **Internet Software** take **longer**.
* **Internet Software & Services** shows the **highest capital efficiency**, generating the **highest valuation relative to funding raised**. Fintech and Mobile & Telecommunications also perform strongly in terms of valuation per funding dollar.
* Year-over-year growth in unicorn creation is **highly uneven**. **Industries like Hardware**, **Supply Chain & Logistics**, **Artificial Intelligence**, and **Fintech** experienced **extremely high growth in 2021**, while some industries showed stagnation or decline in earlier years.
* Unicorn valuations are heavily concentrated within industries. In many sectors, the **top 10% of companies control more than 50% of total industry valuation**, indicating reliance on a small number of dominant players.
* Valuation stability **improved from 2019 to 2020**, but volatility **increased again in 2021**. Although average valuations fell, market uncertainty rose as more companies entered the unicorn category.

<br>

## BUSINESS RECOMMENDATIONS

* Focus investments on industries with **strong growth and high capital efficiency**, such as Internet Software & Services and Fintech, as they generate higher value with relatively lower funding.
* **Avoid over-concentration** in industries where **valuation is dominated by a few companies**, as this increases risk if top firms underperform.
* Balance the portfolio between fast-scaling industries (e.g., AI, Supply Chain & Logistics) and more mature sectors to manage volatility.
* Closely monitor valuation trends during rapid growth periods, as increasing unicorn counts do not always mean higher average valuations.
* Use time-to-unicorn insights to align investment horizons with industry maturity cycles and expected return timelines.

<br>

## FINAL NOTES

This analysis provides a structured view of the unicorn ecosystem from an investment perspective. By combining growth trends, valuation patterns, capital efficiency, and stability metrics, the project highlights both opportunities and risks across industries. The insights help support data-driven portfolio decisions and demonstrate how advanced SQL analysis can be applied to real-world investment strategies.

<br>

## REFERENCES

* Medium
* [DataCamp](https://app.datacamp.com/)
* [What Is the Average Stock Market Return?](https://www.nerdwallet.com/article/investing/average-stock-market-return)
<br>
