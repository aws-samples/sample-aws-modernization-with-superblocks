---
title: "Creating the Filter Section"
chapter: true
weight: 2
---

Let's add filters to help users interact with the dashboard data.

## Step 1: Create the Filter Container

1. Add a new section:

   - Click '+ Add Section' below the navigation bar
   - A new section with a column component will appear

2. Configure the layout:

   - Click the newly generated section so it's in context
   - Within the properties panel, set the Layout property to "Horizontal"
   - Set "Vertical align" property to "Bottom"

## Step 2: Add Filter Components

1. Add dropdowns:

   - In the new section, click "Add Component"
   - Add two dropdown components:
   - Select the first dropdown component and update the Label value in the properties panel to "Paper Categories"
   - Select the second dropdown component and update the Label value in the properties panel to "Location"
   - Set the width property for both dropdowns to "Fill Parent"

2. Add buttons:

   - Click the section with the two dropdowns so it's in context
   - Right click in the section, select 'Add component' and add two button components
   - Select the second button component and update the Label value in the properities panel to "Reset Filters"

<img src="/images/clean-filter-bar.png" width="1200" height="900" />

{{% notice tip %}}
The horizontal layout ensures all filter components are aligned properly in a single row and removes the need to use CSS or Flexbox to align components.
{{% /notice %}}

## Example

Here's how you create your filter section:
<br>
<img src="/images/gifs/filter-add-components.gif" width="1200" height="900" />

## Next Steps

Next, we'll create the hero stats section to display your key metrics.
