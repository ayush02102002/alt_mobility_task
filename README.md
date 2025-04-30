### - Environment Setup

Successfully installed the **PostgreSQL** database server and **pgAdmin**, and established a secure connection to the local PostgreSQL server using the appropriate credentials.

### - Database Creation

- Connected to the local PostgreSQL server instance using **pgAdmin**.
- Created a new dedicated database named **`alt_mobility_assignment`** to keep the project data organized and isolated for this assignment.

### - Database Schema

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

### - Data Loading

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



