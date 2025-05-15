---
title: "Creating the Inventory Data API"
chapter: true
weight: 2
---

# Creating the Inventory Data API

In this section, we'll build a dynamic API that handles inventory data filtering based on their dropdown selections.

## Inventory API

1. Open the API Builder Tool (CMD/CTRL + U)
2. Click "Add new API"
3. Search for the integration to your database (AWS RDS if applicable)
4. Add the below SQL and click "Run API"

```sql
SELECT * FROM dm_operations.inventory
WHERE ({{Dropdown1?.selectedOptionValue || ''}} = '' OR category_name = {{Dropdown1?.selectedOptionValue || ''}})
  AND ({{Dropdown2?.selectedOptionValue || ''}} = '' OR location_name = {{Dropdown2?.selectedOptionValue || ''}})
```

5. Click the pencil icon next the API name and rename it to "get_inventory_data"

{{% notice tip %}}
The values Dropdown1 and Dropdown2 must equal the dropdown component names in the Properties panel in order for the python step to work. If you renamed those components, update the python step code to match the new names.
{{% /notice %}}

## Connect the Table Component

1. Select the Table component
2. In the Properties panel:
   - Delete values in the "Data" field
   - Add: {{get_inventory_data.response}}

## Set up the Button Actions

1. Select the "Submit" button
2. In the Properties panel, at the bottom of the page:

   - Click the + sign next to onClick underneath "Event handlers"
   - Select "Run APIs"
   - Within the "APIs (executed in parallel)" section, choose "get_inventory_data"

3. Select the "Reset Filters" button
4. In the Properties panel, at the bottom of the page:
   - Click the + sign next to onClick underneath "Event handlers"
   - Select "Reset component to default"
   - For the Component field, select "Dropdown1" from the dropdown (leave the Property field as "Selected Option")
   - Repeat this process again by clicking the + sign and adding another reset action for the "Dropdown2" component (ensuring the "Reset Filters" button is still in context)
5. Click the + sign next to onClick underneath "Event handlers" for the third time
   - Select "Run APIs"
   - Within the "APIs (executed in parallel)" section, choose "get_inventory_data"

With the API now connected to the Table component, the table is now dynamically populated with data based on the selected dropdown options.

## Testing

1. Select different combinations of filters
2. Click Submit to verify filtering works
3. Click Reset to verify all filters clear
4. Verify table updates correctly

{{% notice tip %}}
If data is not populating in the table, try clicking the "Run API" button again in the API Builder Tool.
{{% /notice %}}

## Next Steps

After implementing the inventory API and action buttons, we'll move on to creating the hero stats APIs.
