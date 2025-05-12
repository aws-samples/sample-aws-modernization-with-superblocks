---
title: "Creating the Inventory Data API"
chapter: true
weight: 2
---

# Creating the Inventory Data API

In this section, we'll build a dynamic API that handles inventory data filtering based on their dropdown selections.

## Building the API

1. Open API Builder Tool (CMD/CTRL + U)
2. Click "Add new API"
3. Search for "Python Function" integration
4. Click the pencil icon next to 'API1' and rename the API to "get_inventory_data"

## Python Function Step

1. Add the following Python code to dynamically build the SQL query:
```python
def filter_data(dropdown1_value, dropdown2_value):
    conditions = ["TRUE"]
    if dropdown1_value:
        conditions.append(f"category_name = '{dropdown1_value}'")
    if dropdown2_value:
        conditions.append(f"location_name = '{dropdown2_value}'")
    return f"""
    SELECT *
    FROM dm_operations.inventory
    WHERE {' AND '.join(conditions)}
    """

try:
    dropdown1_value = Dropdown1.selectedOptionValue
except AttributeError:
    dropdown1_value = None

try:
    dropdown2_value = Dropdown2.selectedOptionValue
except AttributeError:
    dropdown2_value = None

query = filter_data(dropdown1_value, dropdown2_value)
return query
```

{{% notice tip %}}
The values Dropdown1 and Dropdown2 must equal the dropdown component names in the Properties panel in order for the python step to work. If you renamed those components, update the python step code to match the new names.
{{% /notice %}}

## Postgres Step

1. Click the plus icon under the Python step to add a Postgres step
2. Add the below code in the Postgres step:
```sql
{{Step1.output}}
```
3. Within the Postgres step, click the settings icon at the top right and turn off Parameterized SQL
4. Click "Run API" to test the entire API

{{% notice warning %}}
In this example, we've turned off parameterized SQL because the SQL query is dynamically generated in the Python step. While parameterized SQL is a great way to protect against SQL injection attacks, we've used a list of dropdown values to generate the SQL code. If you were to use any user input in the SQL query, you should enable parameterized SQL to ensure the security of your application.
{{% /notice %}}

## Connect the API to the Table Component

1. Select the Table component
2. In the Properties panel:
   - Delete values in the "Data" field
   - Add: {{get_inventory_data.response}}

{{% notice tip %}}
If data is not populating in the table, try clicking the "Run API" button again in the API Builder Tool.
{{% /notice %}}


## Set Up Button Actions

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

## Next Steps
After implementing the inventory API, we'll move on to creating the hero stats APIs.
