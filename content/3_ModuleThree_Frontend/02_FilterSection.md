---
title: "Creating the Filter Section"
chapter: true
weight: 2
---

# Creating the Filter Section

Let's add filters to help users interact with the dashboard data.

## Step 1: Create the Filter Container
1. Add a new section:
   - Click '+ Add Section' below the navigation bar
   - A new section with a column component will appear
2. Configure the layout:
   - Set layout to "Horizontal"
   - Set "Vertical align" to "Bottom"

## Step 2: Add Filter Components
1. Add dropdowns:
   - Click "Add Component" in the column
   - Add two dropdown components:
     - First dropdown: Label = "Paper Categories"
     - Second dropdown: Label = "Location"
     - Set both widths to "Fill Parent"
2. Add buttons:
   - Add two button components:
     - First button: Label = "Submit"
     - Second button: Label = "Reset Filters"

{{% notice tip %}}
The horizontal layout ensures all filter components are aligned properly in a single row and removes the need to use CSS or Flexbox to align components.
{{% /notice %}}

## Example

Here's how you create your filter section:
<br>
<img src="/images/gifs/filter-add-components.gif" width="1200" height="900" />

## Next Steps

Next, we'll create the hero stats section to display your key metrics.
