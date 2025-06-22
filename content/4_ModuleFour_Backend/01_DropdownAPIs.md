---
title: "Build Dropdown APIs"
chapter: true
weight: 1
---

Let's create two APIs to populate your filter dropdowns with paper categories and locations.

## Step 1: Create the Categories API

1. Open API Builder:

   - Press CMD/CTRL + U
2. Create the API:

   - Click "Add new API"
   - Select your database integration

3. Add the query:

   ```sql
   SELECT DISTINCT category_name
   FROM dm_operations.inventory;
   ```

4. Test and save:

   - Click "Run API"
   - Click the pencil icon next to 'API1' and rename it to "get_papercategories"

## Step 2: Create the Locations API

1. Create the API:

   - Click "Add new API"
   - Search for and select your database integration (aws-superblocks-rds)

2. Add the query:

   ```sql
   SELECT DISTINCT location_name
   FROM dm_operations.inventory;
   ```

3. Test and save:

   - Click "Run API"
   - Click the pencil icon next to 'API1' and rename it to "get_locations"

## Step 3: Connect Your Dropdowns

1. Close API Builder (CMD/CTRL + U)

2. Configure Categories dropdown:

   - Select the Paper Categories dropdown
   - In Properties panel:
     - Find "Options"
     - Clear placeholder data
     - Add: {{get_papercategories.response}}

3. Configure Locations dropdown:

   - Select the Location dropdown
   - In Properties panel:
     - Find "Options"
     - Clear placeholder data
     - Add: {{get_locations.response}}

{{% notice tip %}}
Test both dropdowns after connecting the APIs. They should show real categories and locations. If no data appears, try clicking "Run API" again and verify your SQL queries.
{{% /notice %}}

## Example

Here's how you connect your dropdown components to the APIs:
![Dropdown Components Example](/images/gifs/dropdown-api-example.gif)

## Next Steps

Now that your dropdowns are working, let's create the dynamic inventory data API.
