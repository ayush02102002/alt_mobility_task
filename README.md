### Environment Setup

Successfully installed the **PostgreSQL** database server and **pgAdmin**, and established a secure connection to the local PostgreSQL server using the appropriate credentials.

### Database Creation

- Connected to the local PostgreSQL server instance using **pgAdmin**.
- Created a new dedicated database named **`alt_mobility_assignment`** to keep the project data organized and isolated for this assignment.

### Database Schema

Within the **`alt_mobility_assignment`** database (under the `public` schema), the following table structures were defined:

#### 1. `customer_orders` Table
- `order_id` (TEXT, **Primary Key**)  
- `customer_id` (INTEGER)  
- `order_date` (TEXT) – *Initially defined as TEXT to accommodate import format*  
- `order_amount` (NUMERIC)  
- `shipping_address` (TEXT)  
- `order_status` (TEXT)  
![image](https://github.com/user-attachments/assets/e519f93f-514d-4227-97f2-b641f537f01c)



