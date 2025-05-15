---
title: "Build Dropdown APIs"
chapter: true
weight: 1
---

# Build Dropdown APIs

In this section, we'll create two APIs to populate the filter dropdowns with paper categories and locations.

## Paper Categories API

1. Open the API Builder Tool (CMD/CTRL + U)
2. Click "Add new API"
3. Search for the integration to your database (AWS RDS if applicable)
4. Copy and paste the below SQL inside the Superblocks editor:

```sql
SELECT DISTINCT category_name
FROM dm_operations.inventory;
```

5. Click "Run API"
6. Click the pencil icon next to 'API1' and rename it to "get_papercategories"

## Location Data API

1. Click "Add new API"
2. Search for the integration to your database (same integration as the Paper Categories API)
3. Copy and paste the below SQL inside the Superblocks editor:

```sql
SELECT DISTINCT location_name
FROM dm_operations.inventory;
```

4. Click "Run API"
5. Click the pencil icon next to 'API1' and rename it to "get_locations"

## Connect APIs to Dropdown Components

1. Hide the API Builder Tool (CMD/CTRL + U)
2. Select the first Dropdown component (Paper Categories)
3. In the Properties panel:

   - Find the "Options" section
   - Delete the placeholder data
   - Add: {{get_papercategories.response}}

4. Select the second Dropdown component (Locations)
5. In the Properties panel:
   - Find the "Options" section
   - Delete the placeholder data
   - Add: {{get_locations.response}}

{{% notice tip %}}
Make sure to test both dropdowns after connecting the APIs. They should populate with actual categories and locations from your database. If data is not showing, ensure that you clicked "Run API' and double-check the output of your SQL queries.
{{% /notice %}}

## Example

Here's how you connect your dropdown components to the APIs:
![Dropdown Components Example](/images/gifs/dropdown-api-example.gif)

## Next Steps

Once the dropdowns are working, we'll move on to creating the dynamic inventory data API.
