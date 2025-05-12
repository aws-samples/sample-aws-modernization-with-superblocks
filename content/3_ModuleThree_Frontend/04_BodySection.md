---
title: "Creating the Body Section"
chapter: true
weight: 4
---

# Creating the Body Section

In this section, we'll build the main body of the dashboard, including the data table and charts.

## Steps

**1. Create the Body Section**
   - Add a new section for the body (automatically adds a column component)
   - Set the section height to "Fill Viewport"
   - Set the column layout to "Horizontal"

**2. Add Container Components**
   - Add 2 container components within the section
   - Configure the left container:
     - Width will automatically be set to "Fluid"
     - Should occupy ¾ of the section width
   - Configure the right container:
     - Set width to "Fill Parent"
   - Update both containers to have "Vertical" layout
   - Set both containers' height to "Fill Parent"

**3. Add the Table Component**
   - Add a Table component to the left container
   - Set the table height to "Fill Parent"
   - Remove some of the default selections for the table component:
     - Remove the header (titled "Users" by default)
     - Remove the search bar
     - Remove the download button

**4. Add Chart Components**
   - Add 2 chart components to the right container
   - Set both charts' height to "Fill Parent"

{{% notice tip %}}
The "Fill Parent" and "Fill Viewport" settings ensure your components use the available space effectively and create a responsive layout.
{{% /notice %}}

## Example
Here's how your body section should look after completion:
![Body Section Example](/images/gifs/body-section-example.gif)

## Next Steps
After completing the body section layout, we'll move on to implementing the slide-out panel in the next module.
