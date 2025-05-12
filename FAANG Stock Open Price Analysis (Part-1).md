# 📈 FAANG Stock Open Price Analysis

## Problem Statement

The Bloomberg terminal is the go-to resource for financial professionals, offering convenient access to a wide array of financial datasets. As a **Data Analyst at Bloomberg**, you have access to historical data on stock performance.

Currently, you're analyzing the **highest and lowest open prices** for each **FAANG** stock by **month over the years**.

## Task

For each FAANG stock, display the **ticker symbol**, the **month and year** (`Mon-YYYY`) with the corresponding **highest and lowest open prices**.  
Ensure that the results are **sorted by ticker symbol**.

---

### 📊 `stock_prices` Table Schema

| Column Name | Type      | Description                                                      |
|-------------|-----------|------------------------------------------------------------------|
| date        | datetime  | The specified date (mm/dd/yyyy) of the stock data.              |
| ticker      | varchar   | The stock ticker symbol (e.g., AAPL) for the corresponding company. |
| open        | decimal   | The opening price of the stock at the start of the trading day. |
| high        | decimal   | The highest price reached by the stock during the trading day.  |
| low         | decimal   | The lowest price reached by the stock during the trading day.   |
| close       | decimal   | The closing price of the stock at the end of the trading day.   |

---

### 🧪 Example Input (sample data for AAPL):

| date                | ticker | open   | high   | low    | close  |
|---------------------|--------|--------|--------|--------|--------|
| 01/31/2023 00:00:00 | AAPL   | 142.28 | 142.70 | 144.34 | 144.29 |
| 02/28/2023 00:00:00 | AAPL   | 146.83 | 147.05 | 149.08 | 147.41 |
| 03/31/2023 00:00:00 | AAPL   | 161.91 | 162.44 | 165.00 | 164.90 |
| 04/30/2023 00:00:00 | AAPL   | 167.88 | 168.49 | 169.85 | 169.68 |
| 05/31/2023 00:00:00 | AAPL   | 176.76 | 177.33 | 179.35 | 177.25 |

---

### ✅ Example Output:

| ticker | highest_mth | highest_open | lowest_mth | lowest_open |
|--------|-------------|--------------|------------|-------------|
| AAPL   | May-2023    | 176.76       | Jan-2023   | 142.28      |

> Apple Inc. (AAPL) achieved its highest opening price of $176.76 in May 2023 and its lowest opening price of $142.28 in January 2023.

---

⚠️ **Note**:  
The dataset you are querying against may have different input and output — this is just an example!

### SQL Query

``` sql

WITH cte1 AS (
    SELECT ticker, MAX(open) AS highest_open
    FROM stock_prices
    GROUP BY ticker
), 
cte2 AS (
    SELECT ticker, MIN(open) AS lowest_open
    FROM stock_prices
    GROUP BY ticker
), 
cte3 AS (
    SELECT * FROM stock_prices
)

SELECT 
    cte1.ticker AS ticker,
    TO_CHAR(high.date, 'Mon-YYYY') AS highest_date,
    cte1.highest_open,
    TO_CHAR(low.date, 'Mon-YYYY') AS lowest_date,
    cte2.lowest_open
FROM cte1
INNER JOIN cte2 ON cte1.ticker = cte2.ticker
LEFT JOIN cte3 AS high ON cte1.ticker = high.ticker AND cte1.highest_open = high.open
LEFT JOIN cte3 AS low ON cte1.ticker = low.ticker AND cte2.lowest_open = low.open order by 1;

