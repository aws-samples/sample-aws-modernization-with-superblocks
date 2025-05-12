---
title: "Building the Hero Stats Section"
chapter: true
weight: 3
---

# Building the Hero Stats Section

In this section, we'll create the hero stats section that displays key metrics for the dashboard.

## Steps

**1. Add the Hero Stats Section**
   - Click "+ Add Section" underneath the filter section (automatically adds a column component)
   - Click the icon next to the "Add Component" button and select "Browse UI Templates"
   - Search for "Hero stats with label below, percentage change beside" and click "Insert"

**2. Configure the Hero Stats Section**
   - The Hero Stats UI template comes with 3 stats by default and we want to add a fourth
   - Select the third tile and copy it (CMD/CTRL + C)
   - Press CMD/CTRL + V to paste it and create the fourth tile
   - Update the section headers to:
     - Total Inventory
     - Low Stock Items
     - Pending Orders
     - YTD Sales
   - Update the units of measurement for each stat (e.g. "$" for "Total Inventory" and "items" for "Low Stock Items")
   - Remove the +/- figures and parent container to simplify the display

## Example
Here's how your hero stats section should look after completion:
![Hero Stats Example](/images/gifs/herostats-add-components.gif)

## Next Steps
After completing the hero stats section, we'll move on to creating the main body section with the data table and charts.
