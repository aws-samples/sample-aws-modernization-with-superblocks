---
title: "Resource Setup"
chapter: true
weight: 2
---

# Resource Setup

## Prerequisites

To complete this workshop, you'll need:

1. A Superblocks Account
2. Database Access
3. A Modern Web Browser (Chrome recommended)

## Step 1: Superblocks Account Setup

**Create a Superblocks Account**
- Go to [superblocks.com](https://www.superblocks.com)
- Click "Login" then "Sign up"
- Use your work email (note: public domains like gmail.com are not allowed)
- Verify your email address

**Already have an account?**
- Go to [superblocks.com](https://www.superblocks.com)
- Click "Login"
- Enter your credentials

## Step 2: Database Configuration
{{% notice info %}}
If you're attending a guided workshop, you'll be provided with database credentials and can skip the RDS setup section below.
{{% /notice %}}

**Option 1: Guided Workshop**

**1. Locate Your Provided Credentials**

- Use the workshop credentials provided by your instructor
- Note down the following:
  - Database host
  - Database name
  - Username
  - Password

**Option 2: Self-Paced Workshop (AWS RDS Setup)**

**1. Create an RDS Instance**

- Go to the AWS Console and navigate to RDS
- Click "Create database"
- Choose PostgreSQL as the engine
- Select "Free tier" template if available
- Configure basic settings:
  - DB instance identifier: `sb-workshop-db`
  - Master username: `postgres`
  - Create a secure master password

**2. Configure Network Settings**

- VPC: Choose your default VPC
- Public access: Yes (for workshop purposes only)
- VPC security group: Create new
  - Allow inbound PostgreSQL (port 5432) from your IP

**3. Configure Database Options**

- Initial database name: `workshop_db`
- Leave other settings as default
- Click "Create database"

**4. Wait for Database Creation (~5-10 minutes)**

- Note down your database endpoint when available
- Save your credentials securely

**Configure Superblocks Connection**

1. In Superblocks, go to Integrations
2. Click "Add Integration"
3. Search for and select "PostgreSQL"
4. Enter your database details:
   - Host: Your RDS endpoint or provided workshop host
   - Port: 5432 (default for Postgres)
   - Database: Your database name
   - Username and password
5. Click "Test Connection" to verify and click "Create"

## Step 3: Generate Mock Data

{{% notice info %}}
If you're in a guided workshop, the mock data will already be populated in your provided database. You can skip this section.
{{% /notice %}}

### Database Structure

Let's set up your database. The setup script will create these tables and views in the `dm_operations` schema:

1. `dm_operations.inventory`

   ```sql
   - category_name (varchar)    -- Paper category (e.g., "Copy Paper", "Card Stock")
   - location_name (varchar)    -- Branch location
   - current_stock (integer)    -- Current quantity in stock
   - reorder_point (integer)    -- Minimum stock level before reorder
   - unit_price (numeric)       -- Price per unit
   ```

2. `dm_operations.orders`

   ```sql
   - order_id (integer)         -- Unique order identifier
   - status (varchar)           -- Order status (e.g., "Pending", "Shipped")
   - order_date (timestamp)     -- When the order was placed
   - total_amount (numeric)     -- Total order value
   ```

3. `dm_operations.sales`
   ```sql
   - sale_id (integer)         -- Unique sale identifier
   - sale_date (timestamp)     -- When the sale occurred
   - location_name (varchar)    -- Branch where sale occurred
   - total_amount (numeric)     -- Total sale value
   ```

4. `dm_operations.inventory_location_status` (View)
   ```sql
   - inventory_id (integer)    -- Unique inventory identifier
   - location_name (text)      -- Branch location
   - current_stock (integer)   -- Current quantity in stock
   - stock_status (text)       -- Status based on stock level
   ...
   ```

5. `dm_operations.pending_orders` (View)
   ```sql
   - inventory_id (integer)    -- Inventory reference
   - location_name (text)      -- Branch location
   - num_orders (integer)      -- Number of pending orders
   - total_quantity_ordered (integer) -- Total items on order
   ...
   ```

6. `dm_operations.sales_velocity` (View)
   ```sql
   - inventory_id (integer)    -- Inventory reference
   - location_name (text)      -- Branch location
   - num_sales (integer)       -- Number of sales
   - total_revenue (decimal)   -- Total revenue
   - daily_velocity (decimal)  -- Average daily sales rate
   ...
   ```

### Setup Steps

**1. Clone the Workshop Repository**

```bash
git clone https://github.com/nvardaro-sb/acme-inc-db-setup.git
cd acme-inc-db-setup
```

**2. Set Up Python Environment**

Create and activate a virtual environment to keep dependencies isolated:

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment

# On macOS/Linux:
source venv/bin/activate

# On Windows:
# venv\Scripts\activate
```

**3. Install Required Dependencies**

```bash
# Install dependencies
pip install -r requirements.txt
```

**4. Configure Database Connection**

- Copy the example .env file:
  ```bash
  cp example.env .env
  ```
- Edit `.env` with your database credentials:
  ``` bash
   DB_USER=
   DB_PASSWORD=
   DB_HOST=
   DB_PORT=5432
   DB_NAME=
  ```

**5. Run the Setup Script**

```bash
   ./setup_database.sh
```

This script will create the necessary tables and populate sample data for the application.

{{% notice warning %}}
The setup script will overwrite existing data in the specified tables. Make sure you're using a fresh database or one dedicated to this workshop.
{{% /notice %}}

## Verify Setup

**1. Test Database Access**

```sql
SELECT COUNT(*)
FROM dm_operations.inventory;
```

{{% notice warning %}}
If you can't connect to the database or access any part of Superblocks, please ask for assistance or reach out to Superblocks support before proceeding.
{{% /notice %}}

## Next Steps

After completing the setup, we'll explore the technical concepts behind our application.
