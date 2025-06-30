---
title: "Database Setup"
chapter: true
weight: 3
---

# Setting Up the ACME Inc Database

In this section, you'll set up the sample ACME Inc database that will be used throughout the workshop. This step is required for both AWS event participants and self-paced learners.

## Prerequisites

- Completed VS Code Server setup
- Database connection environment variables configured

## Database Setup Steps

### Step 1: Clone the Repository

Open a terminal in VS Code Server and run the following commands:

```bash
# Clone the repository
git clone https://github.com/superblocksteam/aws-workshop
cd aws-workshop
```

### Step 2: Set Up Python Environment

Before running the setup script, you need to set up a Python environment:

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate

# On Windows:
# venv\Scripts\activate

# The environment variables should already be set from the previous section
echo "DB_USER=$DB_USERNAME" > .env
echo "DB_PASSWORD=$DB_PASSWORD" >> .env
echo "DB_HOST=$DB_ENDPOINT" >> .env
echo "DB_PORT=5432" >> .env
echo "DB_NAME=$DB_NAME" >> .env
```

### Step 3: Run the Setup Script

The repository contains a script that will set up the ACME Inc database with sample data:

```bash
# Make the script executable
chmod +x setup_db.sh

# Run the setup script
./setup_db.sh
```

This script will:
1. Connect to the existing RDS instance that was created by the CloudFormation template
2. Create the necessary tables and schema for the ACME Inc application
3. Import sample data for inventory, orders, and sales
4. Set up the required permissions and create the necessary views

### Step 3: Verify the Database Setup

To verify that the database has been set up correctly, run the following command:

```bash
# Connect to the database and count the number of inventory items
psql -h $DB_ENDPOINT -U $DB_USERNAME -d $DB_NAME -c "SELECT COUNT(*) FROM dm_operations.inventory;"
```

You should see a count of the inventory items in the database. If you see a number (typically several dozen records), the database has been set up successfully.

### Step 4: Explore the Database Schema

The ACME Inc database has the following schema in the `dm_operations` tables:

```
dm_operations (schema)
├── inventory
│   ├── inventory_id (PK)      -- Unique inventory identifier
│   ├── category_name          -- Paper category (e.g., "Copy Paper", "Card Stock")
│   ├── location_name          -- Branch location
│   ├── current_stock          -- Current quantity in stock
│   ├── reorder_point          -- Minimum stock level before reorder
│   └── unit_price             -- Price per unit
├── orders
│   ├── order_id (PK)          -- Unique order identifier
│   ├── status                 -- Order status (e.g., "Pending", "Shipped")
│   ├── order_date             -- When the order was placed
│   └── total_amount           -- Total order value
├── sales
│   ├── sale_id (PK)           -- Unique sale identifier
│   ├── sale_date              -- When the sale occurred
│   ├── location_name          -- Branch where sale occurred
│   └── total_amount           -- Total sale value
├── inventory_location_status (View)
│   ├── inventory_id           -- Unique inventory identifier
│   ├── location_name          -- Branch location
│   ├── current_stock          -- Current quantity in stock
│   ├── stock_status           -- Status based on stock level
│   └── ...                    -- Additional derived fields
├── pending_orders (View)
│   ├── inventory_id           -- Inventory reference
│   ├── location_name          -- Branch location
│   ├── num_orders             -- Number of pending orders
│   ├── total_quantity_ordered -- Total items on order
│   └── ...                    -- Additional derived fields
└── sales_velocity (View)
    ├── inventory_id           -- Inventory reference
    ├── location_name          -- Branch location
    ├── num_sales              -- Number of sales
    ├── total_revenue          -- Total revenue
    ├── daily_velocity         -- Average daily sales rate
    └── ...                    -- Additional derived fields
```

This database will be used throughout the workshop to build your Superblocks application.

## Troubleshooting

If you encounter any issues during the database setup:

1. **Connection Issues**:

   - Verify that your environment variables are set correctly
   - Check that the RDS instance is running and accessible
   - Ensure your security group allows connections from the VS Code Server

2. **Permission Issues**:

   - Verify that you have the correct database credentials
   - Check that the database user has the necessary permissions

3. **Script Execution Issues**:

   - Make sure the script is executable (`chmod +x setup_db.sh`)
   - Check for any error messages in the script output

## Next Steps

Now that you have set up the ACME Inc database, you're ready to proceed to [Module 1: Overview](/1_ModuleOne_Overview) and start building your Superblocks application.
