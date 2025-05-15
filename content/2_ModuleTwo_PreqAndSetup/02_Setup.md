---
title: "Workshop Resource Setup"
chapter: true
weight: 2
---

# Workshop Resource Setup

## Prerequisites

Before starting this workshop, you'll need:

1. A Superblocks Account
2. Database Access
3. Web Development Environment (Chrome recommended)

## Getting Started

### Superblocks Account Setup

**1. Create a Superblock's Account (if you don't have one)**

- Visit the Superblocks website [superblocks.com](https://www.superblocks.com)
- Click "Try for free" at the top of the page
- Sign up with your work email (public email domains such as gmail, yahoo, outlook, etc. are blocked)
- Verify your email address

**2. Login to Superblocks (already have an account)**

- Go to the Superblocks website [superblocks.com](https://www.superblocks.com)
- Click "Login" at the top of the page
- Login with your credentials

### Database Configuration

{{% notice info %}}
If you're attending a guided workshop, you'll be provided with database credentials and can skip the RDS setup section below.
{{% /notice %}}

### Option 1: Guided Workshop

**1. Locate Your Provided Credentials**

- Use the workshop credentials provided by your instructor
- Note down the following:
  - Database host
  - Database name
  - Username
  - Password

### Option 2: Self-Paced Workshop (AWS RDS Setup)

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

### Configure Superblocks Connection

1. In Superblocks, go to Integrations
2. Click "Add Integration"
3. Select "PostgreSQL"
4. Enter your database details:
   - Host: Your RDS endpoint or provided workshop host
   - Port: 5432 (default for Postgres)
   - Database: Your database name
   - Username and password
5. Click "Test Connection" to verify and click "Create"

## Setup Mock Data

{{% notice info %}}
If you're in a guided workshop, the mock data will already be populated in your provided database. You can skip this section.
{{% /notice %}}

### Database Structure

The setup script will create three main tables:

1. `dm_operations.inventory`

   ```sql
   - category_name (text)      -- Paper category (e.g., "Copy Paper", "Card Stock")
   - location_name (text)      -- Branch location
   - current_stock (integer)   -- Current quantity in stock
   - reorder_point (integer)   -- Minimum stock level before reorder
   - unit_price (decimal)      -- Price per unit
   ```

2. `dm_operations.orders`

   ```sql
   - order_id (integer)        -- Unique order identifier
   - status (text)             -- Order status (e.g., "Pending", "Shipped")
   - order_date (timestamp)    -- When the order was placed
   - total_amount (decimal)    -- Total order value
   ```

3. `dm_operations.sales`
   ```sql
   - sale_id (integer)         -- Unique sale identifier
   - sale_date (timestamp)     -- When the sale occurred
   - location_name (text)      -- Branch where sale occurred
   - total_amount (decimal)    -- Total sale value
   ```

### Setup Steps

**1. Clone the Workshop Repository**

```bash
git clone https://github.com/superblocks-at/workshop-mock-data.git
cd workshop-mock-data
```

**2. Install Required Dependencies**

```bash
pip install -r requirements.txt
```

**3. Configure Database Connection**

- Copy the example configuration file:
  ```bash
  cp config.example.py config.py
  ```
- Edit `config.py` with your database credentials:
  ```python
  DB_CONFIG = {
      'host': 'your-db-endpoint',
      'database': 'workshop_db',
      'user': 'your-username',
      'password': 'your-password'
  }
  ```

**4. Run the Setup Script**

```bash
python setup_mock_data.py
```

This will:

- Create necessary tables
- Populate sample inventory data
- Add mock transaction history
- Generate test user accounts

{{% notice warning %}}
The setup script will overwrite existing data in the specified tables. Make sure you're using a fresh database or one dedicated to this workshop.
{{% /notice %}}

## Verify Setup

**1. Test Database Access**

```sql
SELECT COUNT(*)
FROM dm_operations.inventory;
```

**2. Verify Platform Access**

- Access to Application Builder
- Ability to create new APIs
- Permission to query the database

{{% notice warning %}}
If you can't connect to the database or access any part of Superblocks, please ask for assistance or reach out to Superblocks support before proceeding.
{{% /notice %}}

## Next Steps

After completing the setup, we'll explore the technical concepts behind our application.
