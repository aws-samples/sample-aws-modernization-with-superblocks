---
title: "Creating the Filter Section"
chapter: true
weight: 2
---

# Creating the Filter Section

In this section, we'll build the filter components that allow users to interact with the dashboard data.

## Steps

**1. Add the Filter Section**

- Click '+ Add Section' below the navigation bar (will automatically create a new section with a column component)
- Set the layout of the column component to "Horizontal"
- Set the "Vertical align" property of the column component to "Bottom"

**2. Create the Filter Container**

- Click the column component and within the column component click "Add Component"
- Search for "Dropdown" and add two dropdown components
- Search for "Button" and add two button components
- Configure the dropdown components:
  - Update the Label property of the first dropdown to "Paper Categories"
  - Update the Label property of the second dropdown to "Location"
  - Set the width for both dropdowns to "Fill Parent"
- Configure the button components:
  - Update the Label property of the first button to "Submit"
  - Update the Label property of the second button to "Reset Filters"

{{% notice tip %}}
The horizontal layout ensures all filter components are aligned properly in a single row and removes the need to use CSS or Flexbox to align components.
{{% /notice %}}

## Example

Here's how you create your filter section:
<br>
<img src="/images/gifs/filter-add-components.gif" width="1200" height="900" />

## Next Steps

After completing the filter section, we'll move on to creating the hero stats section to display key performance indicators.
