### **Name:** Ayush Katkurwar  
### **Phone:** +91-9022496085  
### **Email:** ayushkatkurwar3@gmail.com  

### **Alt-Mobility Task for Data Analyst Intern Role:**


### &#8226; Environment Setup

Successfully installed the **PostgreSQL** database server and **pgAdmin**, and established a secure connection to the local PostgreSQL server using the appropriate credentials.

### &#8226; Database Creation

- Connected to the local PostgreSQL server instance using **pgAdmin**.
- Created a new dedicated database named **`alt_mobility_assignment`** to keep the project data organized and isolated for this assignment.

### &#8226; Database Schema

Within the **`alt_mobility_assignment`** database (under the `public` schema), the following table structures were defined:

#### 1. `customer_orders` Table
- `order_id` (TEXT, **Primary Key**)  
- `customer_id` (INTEGER)  
- `order_date` (TEXT) – *Initially defined as TEXT to accommodate import format*  
- `order_amount` (NUMERIC)  
- `shipping_address` (TEXT)  
- `order_status` (TEXT)
  
<img src="https://github.com/user-attachments/assets/e519f93f-514d-4227-97f2-b641f537f01c" style="margin-left: 40px;" width="400"/>

#### 2. `payments` Table
- `payment_id` (TEXT, **Primary Key**)  
- `order_id` (TEXT, **Foreign Key** referencing `customer_orders.order_id`)  
- `payment_date` (TEXT) – *Initially defined as TEXT to accommodate import format*  
- `payment_amount` (NUMERIC)  
- `payment_method` (TEXT)  
- `payment_status` (TEXT)  

<img src="https://github.com/user-attachments/assets/c1fe3e2c-6d1b-4909-bd60-e71c7b457dfd" width="400"/>


**Constraints Applied:**
- Primary Key constraints on `order_id` and `payment_id`  
- Foreign Key constraint linking `payments.order_id` to `customer_orders.order_id`

### &#8226; Data Loading

Data was imported into the PostgreSQL database using **pgAdmin**:

1. Navigated to the desired table (e.g., `customer_orders`, `payments`) in pgAdmin.
2. Used the **"Import/Export Data..."** feature.
3. Configured the import settings:
   - Selected the appropriate CSV file:  
     - `customer_orders.csv`  
     - `payments.csv`
   - Set the format to **CSV**.
   - Enabled the **"Header"** option.
   - Confirmed the **delimiter** was set to a comma (`,`).
4. Successfully loaded the data into the respective tables.

<img src="https://github.com/user-attachments/assets/06f58d42-14d8-4b89-8682-5d50fe2ced90" width="400"/>
<img src="https://github.com/user-attachments/assets/468da3fc-6791-42c5-9a6d-19d3823b51b7" width="400"/>

### • Initial Data Inspection & Verification

- Used the **pgAdmin Query Tool** to run basic SQL commands for initial data checks:
  - Viewed sample rows using:  
    `SELECT * FROM table_name LIMIT 10;`
  - Checked total row counts:  
    `SELECT COUNT(*) FROM table_name;`
  - Investigated unique values in key categorical columns:
    - `SELECT DISTINCT order_status FROM customer_orders;`
    - `SELECT DISTINCT payment_status FROM payments;`
    - `SELECT DISTINCT payment_method FROM payments;`
  - Checked for **NULL values** in essential columns.
  - Verified the **uniqueness of primary keys** to ensure data integrity.

### • Data Type Conversion (Critical Date Handling)

- Confirmed that although the original CSVs used the **DD-MM-YYYY** date format, the import process resulted in `order_date` and `payment_date` being stored as **text** in the format **YYYY-MM-DD**.
- Added temporary columns with the correct `DATE` data type:
  - `order_date_dt` in the `customer_orders` table
  - `payment_date_dt` in the `payments` table
- Populated the new `DATE` columns by casting the existing text columns:
  
  ```sql
  UPDATE customer_orders
  SET order_date_dt = CAST(order_date AS DATE);

  UPDATE payments
  SET payment_date_dt = CAST(payment_date AS DATE);

- Compared the old text columns with the new date columns to ensure the values matched.
- Dropped the original text-based date columns:
  - `order_date` from `customer_orders`
  - `payment_date` from `payments`

- Renamed the new DATE columns back to the original names:
  ```sql
  ALTER TABLE customer_orders
  RENAME COLUMN order_date_dt TO order_date;

  ALTER TABLE payments
  RENAME COLUMN payment_date_dt TO payment_date;

### 📝 Task 1: Order and Sales Analysis

**🎯 Objective:**  
Analyze order status and sales data to provide insights into order fulfillment and revenue trends. Identify key metrics and trends related to order status and sales.

This task involves looking at two main aspects:

- **Order Fulfillment**: Understanding the lifecycle and status of orders placed.  
- **Sales & Revenue**: Understanding the monetary value generated and its patterns over time.

#### 1. Analyzing Order Fulfillment Status

Understanding the status distribution of orders helps assess **operational efficiency**.  
- A high number of `'pending'` or `'shipped'` (but not `'delivered'`) orders might indicate **processing bottlenecks**.  
- Comparing `'delivered'` to other final states (like `'canceled'`, if present) provides insight into the **completion rate**.

**🔍 SQL Query:**

```sql
SELECT
    order_status,
    COUNT(*) AS number_of_orders,
    ROUND((COUNT(*) * 100.0 / SUM(COUNT(*)) OVER ()), 2) AS percentage_of_total_orders
FROM
    customer_orders
GROUP BY
    order_status
ORDER BY
    number_of_orders DESC;
```
<img src="https://github.com/user-attachments/assets/27f57bbc-9995-4a80-8269-a9ac05b2e618" width="450"/>

#### 2. Analyzing Sales Revenue

**🔍 SQL Query:**

```sql
SELECT
    SUM(payment_amount) AS total_revenue
FROM
    payments
WHERE
    LOWER(payment_status) = 'completed';
```

Provides insight into the **overall top-line revenue** based on **successful (completed)** transactions.  
This helps evaluate the **financial performance** of the system.

<img src="https://github.com/user-attachments/assets/5c196bf1-ad8e-4416-8308-96b81c5ae1d0" width="450"/>


**🔍 SQL Query:**

```sql
SELECT 
    TO_CHAR(payment_date, 'YYYY-MM') AS payment_month,
    SUM(payment_amount) AS monthly_revenue
FROM
    payments
WHERE
    LOWER(payment_status) = 'completed'
GROUP BY
    payment_month
ORDER BY
    payment_month;
```

Identifies growth patterns, seasonality, or declines in sales performance. Crucial for business health monitoring.

<img src="https://github.com/user-attachments/assets/cd4d384c-78e0-42bb-bbc0-859982163f12" width="450"/>

#### 3. Identifying Key Sales Metrics

**🔍 SQL Query:**

```sql
SELECT
    AVG(payment_amount) AS average_order_value
FROM
    payments
WHERE
    LOWER(payment_status) = 'completed';
```

Measures the average revenue generated per successful transaction. Helps understand purchasing behavior and optimize pricing/promotions.

<img src="https://github.com/user-attachments/assets/ccc1b5c3-4a8f-4d07-a446-68e1904053f1" width="450"/>

**🔍 SQL Query:**

```sql
WITH CompletedOrderTotals AS (
    SELECT
        order_id,
        SUM(payment_amount) AS total_revenue_for_order
    FROM
        payments
    WHERE
        LOWER(payment_status) = 'completed'
    GROUP BY
        order_id
)
SELECT
    AVG(total_revenue_for_order) AS average_revenue_per_completed_order
FROM
    CompletedOrderTotals;
```

Slightly different perspective, focusing on the value derived per unique customer order that was successfully paid for.

<img src="https://github.com/user-attachments/assets/ec39d6c0-dca7-43f2-b61d-b18f64aaf5ec" width="450">

#### 4. Analyzing Trends Related to Order Status

**🔍 SQL Query:**

```sql
SELECT
    TO_CHAR(order_date, 'YYYY-MM') AS order_placement_month,
    COUNT(order_id) AS number_of_orders_placed
FROM
    customer_orders
GROUP BY
    order_placement_month
ORDER BY
    order_placement_month;
```

Shows the trend in customer activity or demand initiation. 

<img src="https://github.com/user-attachments/assets/7450fc6c-a568-4a41-8343-4de49e5ef8d0" width="450"/>

### 📝 Task 2: Customer Analysis

**🎯 Objective:** Explore customer ordering behavior to identify patterns such as repeat ordering, customer segmentation based on frequency, and trends over time.

#### 1. Identifying Repeat Customers

**🔍 SQL Query:**

```sql
SELECT
    customer_id,
    COUNT(order_id) AS number_of_orders
FROM
    customer_orders
GROUP BY
    customer_id 
HAVING
    COUNT(order_id) > 1
ORDER BY
    number_of_orders DESC;
```

This identifies your loyal or engaged customer base. Understanding the size and purchasing frequency of this group is vital for retention strategies.

<img src="https://github.com/user-attachments/assets/f5a524cb-111f-4d08-b6a3-cfe2642f577e" width="450"/>

#### 2. Customer Segmentation by Order Frequency

**🔍 SQL Query:**

```sql
WITH CustomerOrderCounts AS (
    SELECT
        customer_id,
        COUNT(order_id) AS order_count
    FROM
        customer_orders
    GROUP BY
        customer_id
)
SELECT
    order_count,
    COUNT(customer_id) AS number_of_customers
FROM
    CustomerOrderCounts
GROUP BY
    order_count
ORDER BY
    order_count;
```

This provides a simple but powerful behavioral segmentation. It shows the distribution of your customer base from one-time buyers to highly frequent purchasers. This helps understand the overall loyalty structure.

<img src="https://github.com/user-attachments/assets/2d9a613d-5424-413b-9ec7-e19848503ccd" width="450"/>

#### 3. Analyzing Customer Activity Trends Over Time

**🔍 SQL Query:**

```sql
WITH CustomerFirstOrder AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_order_date
    FROM customer_orders
    GROUP BY customer_id
),
MonthlyOrders AS (
     SELECT
        customer_id,
        TO_CHAR(order_date, 'YYYY-MM') AS order_month,
        order_date
     FROM customer_orders
)
SELECT
    mo.order_month,
    COUNT(DISTINCT mo.customer_id) AS total_active_customers,
    COUNT(DISTINCT CASE WHEN mo.order_date = cfo.first_order_date THEN mo.customer_id ELSE NULL END) AS new_customers_this_month, -- Customers whose first order was this month
    COUNT(DISTINCT CASE WHEN mo.order_date > cfo.first_order_date THEN mo.customer_id ELSE NULL END) AS returning_customers_this_month -- Customers active this month but whose first order was earlier
FROM MonthlyOrders mo
JOIN CustomerFirstOrder cfo ON mo.customer_id = cfo.customer_id
GROUP BY mo.order_month
ORDER BY mo.order_month;
```

Tracks overall customer engagement monthly. Distinguishing between new and returning customers helps understand if growth is driven by acquisition or retention.

<img src="https://github.com/user-attachments/assets/49648edf-59bf-4370-8a0e-a796ddcd7c54" width="450" />

### 📝 Task 3: Payment Status Analysis

**🎯 Objective:** Investigate payment status data to identify potential issues or trends related to payment success and failure.

#### 1. Overall Payment Status Distribution

**🔍 SQL Query:**

```sql
SELECT
    payment_status,
    COUNT(*) AS count_status,
    ROUND((COUNT(*) * 100.0 / SUM(COUNT(*)) OVER ()), 2) AS percentage_status
FROM
    payments
GROUP BY
    payment_status
ORDER BY
    count_status DESC;
```

Provides a high-level view of payment processing health. A high failure or pending rate signals potential problems needing investigation.

<img src="https://github.com/user-attachments/assets/01ae6a0d-70c2-499a-aac1-8d0d7c969032" width="450"/>

#### 2. Payment Status Breakdown by Payment Method

**🔍 SQL Query:**

```sql
SELECT
    payment_method,
    payment_status,
    COUNT(*) AS count_status
FROM
    payments
GROUP BY
    payment_method, payment_status
ORDER BY
    payment_method, count_status DESC;
```

Helps pinpoint if issues are widespread or isolated to specific payment processors or types (e.g., credit cards failing more often than bank transfers).

<img src="https://github.com/user-attachments/assets/24ba6e9d-8aab-40e5-af38-83ef0cb7c988" width="450" />

#### 3. Trend Analysis of Payment Failures

**🔍 SQL Query:**

```sql
SELECT
    TO_CHAR(payment_date, 'YYYY-MM') AS payment_month,
    COUNT(*) AS failed_payment_count
FROM
    payments
WHERE
    LOWER(payment_status) = 'failed'
GROUP BY
    payment_month
ORDER BY
    payment_month;
```

Identifies whether payment problems are worsening systemically or perhaps improving after interventions. Can reveal seasonality in failures or impacts from external events/changes.

<img src="https://github.com/user-attachments/assets/56ecf7f7-eb9d-47c1-aba5-b685d9e1b566" width="450" />


**🔍 SQL Query:**

```sql
SELECT
    payment_month,
    total_payments,
    failed_payments,
    CASE
        WHEN total_payments = 0 THEN 0
        ELSE ROUND((failed_payments * 100.0 / total_payments), 2)
    END AS failure_rate_percent
FROM (
    SELECT
        TO_CHAR(payment_date, 'YYYY-MM') AS payment_month,
        COUNT(*) AS total_payments,
        COUNT(*) FILTER (WHERE LOWER(payment_status) = 'failed') AS failed_payments
    FROM payments
    GROUP BY payment_month
) AS MonthlyCounts 
ORDER BY payment_month;
```

This gives better context than just the count, as it accounts for changes in overall transaction volume.

<img src="https://github.com/user-attachments/assets/d0c574e7-3f4b-42f3-a91c-0648eeab4060" alt="image" />

### 📝 Task 4: Order Details Report Query

**🎯 Objective:** Create a comprehensive SQL query that provides a detailed, row-level overview of order information, payment details, and potentially some calculated row-level metrics if applicable (though the prompt primarily implies joining existing details).

**🔍 SQL Query:**

```sql
SELECT
    co.order_id,
    co.customer_id,
    co.order_date,         
    co.order_amount,        
    co.order_status,        
    co.shipping_address,
    p.payment_id,
    p.payment_date,  
    p.payment_amount AS payment_attempt_amount,
    p.payment_method,
    p.payment_status,
    CASE
        WHEN LOWER(p.payment_status) = 'completed' AND p.payment_amount = co.order_amount THEN 'Match'
        WHEN LOWER(p.payment_status) = 'completed' AND p.payment_amount <> co.order_amount THEN 'Mismatch'
        ELSE 'N/A'
    END AS payment_order_amount_match_status,
    CASE
        WHEN p.payment_date IS NOT NULL THEN p.payment_date - co.order_date 
        ELSE NULL
    END AS days_between_order_and_payment
FROM
    customer_orders co
LEFT JOIN
    payments p ON co.order_id = p.order_id 
ORDER BY
    co.order_date DESC, 
    co.order_id,     
    p.payment_date;
```

<img src="https://github.com/user-attachments/assets/5d44f1b6-9cd8-43da-ac0a-ec3aaf7a98f6" width="450" />




