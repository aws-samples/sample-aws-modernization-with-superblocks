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
# Create a workshop directory
mkdir -p ~/Workshop
cd ~/Workshop

# Clone the repository
git clone https://github.com/aws-samples/acme-inc-db-setup.git
cd acme-inc-db-setup
```

### Step 2: Run the Setup Script

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
3. Import sample data for products, customers, orders, and order items
4. Set up the required permissions

### Step 3: Verify the Database Setup

To verify that the database has been set up correctly, run the following command:

```bash
# Connect to the database and count the number of products
psql -h $DB_ENDPOINT -U $DB_USERNAME -d $DB_NAME -c "SELECT COUNT(*) FROM products;"
```

You should see a count of the products in the database. If you see a number (typically several dozen records), the database has been set up successfully.

### Step 4: Explore the Database Schema

The ACME Inc database has the following schema:

```
acme (database)
├── products
│   ├── product_id (PK)
│   ├── name
│   ├── description
│   ├── price
│   ├── category
│   └── inventory_count
├── customers
│   ├── customer_id (PK)
│   ├── first_name
│   ├── last_name
│   ├── email
│   └── registration_date
├── orders
│   ├── order_id (PK)
│   ├── customer_id (FK)
│   ├── order_date
│   ├── status
│   └── total_amount
└── order_items
    ├── order_item_id (PK)
    ├── order_id (FK)
    ├── product_id (FK)
    ├── quantity
    └── price
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
