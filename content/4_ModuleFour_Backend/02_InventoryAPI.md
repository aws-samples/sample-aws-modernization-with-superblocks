---
title: "Creating the Inventory Data API"
chapter: true
weight: 2
---

# Creating the Inventory Data API

Let's create a dynamic API that filters inventory data based on your dropdown selections.

## Step 1: Create the Inventory API
1. Open API Builder:
   - Press CMD/CTRL + U
2. Create the API:
   - Click "Add new API"
   - Select your database integration
3. Add the query:
   ```sql
   SELECT * FROM dm_operations.inventory
   WHERE ({{Dropdown1?.selectedOptionValue || ''}} = '' OR 
          category_name = {{Dropdown1?.selectedOptionValue || ''}}) AND
         ({{Dropdown2?.selectedOptionValue || ''}} = '' OR 
          location_name = {{Dropdown2?.selectedOptionValue || ''}});
   ```
4. Test and save:
   - Click "Run API"
   - Click the pencil icon next to "API1" and rename it to "get_inventory_data"

{{% notice tip %}}
Make sure your dropdown component names match `Dropdown1` and `Dropdown2`. If you used different names, update them in the SQL query.
{{% /notice %}}

## Step 2: Connect Your Table
1. Configure the table:
   - Select the Table component
   - In Properties panel:
     - Clear the "Data" field
     - Add: {{get_inventory_data.response}}

## Step 3: Set Up Actions
1. Configure Submit button:
   - Select the "Submit" button
   - Add onClick handler:
     - Click + next to "Event handlers"
     - Choose "Run APIs"
     - Select "get_inventory_data"

2. Configure Reset button:
   - Select the "Reset Filters" button
   - Add first reset action:
     - Click + next to "Event handlers"
     - Choose "Reset component to default"
     - Select "Dropdown1" (keep "Selected Option")
   - Add second reset action:
     - Click + again
     - Choose "Reset component to default"
     - Select "Dropdown2" (keep "Selected Option")
   - Add refresh action:
     - Click + again
     - Choose "Run APIs"
     - Select "get_inventory_data"

Your table will now update dynamically based on the selected filters.

## Testing
1. Test filtering:
   - Try different filter combinations
   - Click Submit to apply filters
   - Verify data updates correctly
2. Test reset:
   - Click Reset Filters
   - Verify dropdowns clear
   - Check if table refreshes

{{% notice tip %}}
If the table is empty, try clicking "Run API" in the API Builder Tool.
{{% /notice %}}

## Next Steps
Now that your inventory API is working, let's create the hero stats APIs.
