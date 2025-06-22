---
title: "Implementing the Hero Stats APIs"
chapter: true
weight: 3
---

Let's create APIs for your hero stats using parallel execution to fetch multiple metrics simultaneously.

## Step 1: Set Up the API

1. Open API Builder:

   - Press CMD/CTRL + U

2. Create parallel API:

   - Click "Add new API"
   - Select "Control Blocks"
   - Select "Run Parallel"
   - Click the pen icon next to 'API1' and rename it to "get_herostats"

## Step 2: Configure Parallel Paths

1. Add paths:

   - Click the Parallel block
   - Click + to add two new paths

2. Name your paths:

   - Path1: "get_inventory"
   - Path2: "get_lowstock"
   - Path3: "get_pendingorders"
   - Path4: "get_ytdsales"

## Step 3: Add Your Queries

Click “+ Add Block” for each path, search for and select your database integration (aws-superblocks-rds), and add these SQL queries to each path:

1. Total Inventory Value (Path1):

   ```sql
   SELECT SUM(current_stock * unit_price)
   FROM dm_operations.inventory;
   ```

2. Low Stock Items (Path2):

   ```sql
   SELECT COUNT(*)
   FROM dm_operations.inventory
   WHERE current_stock <= reorder_point;
   ```

3. Pending Orders (Path3):

   ```sql
   SELECT COUNT(*)
   FROM dm_operations.orders
   WHERE status = 'Pending';
   ```

4. YTD Sales (Path4):

   ```sql
   SELECT SUM(total_amount)
   FROM dm_operations.sales
   WHERE EXTRACT(YEAR FROM sale_date) = 
         EXTRACT(YEAR FROM CURRENT_DATE);
   ```

## Step 4: Connect Your Stats

Update each stat with its API response:

1. Total Inventory:

   ```sh
   {{get_herostats.response.get_inventory}}
   ```

2. Low Stock Items:

   ```sh
   {{get_herostats.response.get_lowstock}}
   ```

3. Pending Orders:

   ```sh
   {{get_herostats.response.get_pendingorders}}
   ```

4. YTD Sales:

   ```sh
   {{get_herostats.response.get_ytdsales}}
   ```

## Testing

1. Check your stats:

   - Run the API
   - Verify all numbers appear
   - Check formatting

{{% notice tip %}}
If stats are missing, click "Run API" in the API Builder Tool.
{{% /notice %}}

## Next Steps

Let's create the Plotly visualization APIs for your charts.
