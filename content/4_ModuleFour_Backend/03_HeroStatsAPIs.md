---
title: "Implementing the Hero Stats APIs"
chapter: true
weight: 3
---

# Implementing the Hero Stats APIs

In this section, we'll create APIs to power the hero stats section and use the Parallel block to run sets of API blocks at the same time. This is a powerful block that unlocks the ability to parallelize work in your APIs.

## Hero Stats API

1. Open the API Builder Tool (CMD/CTRL + U)
2. Click "Add new API"
3. Hover over "Control Blocks" and click "Run Parallel"
4. Click the pencil icon next to 'API1' and rename the API to "get_herostats"

## Configure Paths

With a parallel control block, you can run multiple API blocks at the same time. This is a powerful feature that allows you to parallelize work in your APIs.

1. Click on the Parallel block
2. Click the plus icon under the Parallel block to add two new paths
3. Update the "Path Names" to:

   - Path1 = "get_inventory"
   - Path2 = "get_lowstock"
   - Path3 = "get_pendingorders"
   - Path4 = "get_ytdsales"

Within each Path, select the block and add the database integration. Add the below SQL to each block to pull data in parallel for the respective hero stats:

### Total Inventory Value

```sql
SELECT SUM(current_stock * unit_price)
FROM dm_operations.inventory;
```

### Low Stock Items Count

```sql
SELECT COUNT(*)
FROM dm_operations.inventory
WHERE current_stock <= reorder_point;
```

### Pending Orders Count

```sql
SELECT COUNT(*)
FROM dm_operations.orders
WHERE status = 'Pending';
```

### Year-to-Date Sales

```sql
SELECT SUM(total_amount)
FROM dm_operations.sales
WHERE EXTRACT(YEAR FROM sale_date) = EXTRACT(YEAR FROM CURRENT_DATE);
```

## Connect the Hero Stats Components

Update each hero stat component with the corresponding API response. You can access the output of the Parallel block by referencing its path name (e.g., api_name.response.path_name).

1. Total Inventory:

```
{{get_herostats.response.get_inventory}}
```

2. Low Stock Items:

```
{{get_herostats.response.get_lowstock}}
```

3. Pending Orders:

```
{{get_herostats.response.get_pendingorders}}
```

4. YTD Sales:

```
{{get_herostats.response.get_ytdsales}}
```

## Testing

1. Verify all stats load correctly
2. Confirm proper formatting of values

{{% notice tip %}}
If data is not populating in the table, try clicking the "Run API" button again in the API Builder Tool.
{{% /notice %}}

## Next Steps

Now we'll move on to creating the Plotly visualization APIs for our charts.
