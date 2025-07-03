---
title: "Building the Hero Stats Section"
chapter: true
weight: 3
---

Let's add key metrics to your dashboard using hero stats.

## Step 1: Add the Stats Section

1. Create a new section:

   - Click "+ Add Section" below the filters
   - A column component will be added automatically

2. Add the template:

   - In the newly created section, click the template icon next to "Add Component"
   - Search for "Hero stats with label below, percentage change beside"
   - Click "Insert"

## Step 2: Configure Your Stats

1. Add a fourth stat:

   - Select the third stat tile and copy it (CMD/CTRL + C)
   - Paste to create a new tile (CMD/CTRL + V)

2. Update the stats

   - Rename each hero stat tile and set its data type in the properties panel:

     1. Change "Total subscribers" to "Total Inventory"
        - Set data type to Currency
     2. Change "Average open rate" to "Low Stock Items"
        - Set data type to Number
     3. Change "Average click rate" to "Pending Orders"
        - Set data type to Number
     4. Change "Average click rate" to "YTD Sales"
        - Set data type to Currency

3. Simplify the display:

   - Remove the plus/minus components, percentage components, and parent containers

![Hero Stats](/images/hero-stats.png)

## Example

Here's how your hero stats section should look after completion:
![Hero Stats Example](/images/gifs/herostats-add-components.gif)

## Next Steps

Next, we'll create the main dashboard section with your data table and charts.
