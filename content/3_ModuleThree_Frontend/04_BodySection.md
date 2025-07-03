---
title: "Creating the Body Section"
chapter: true
weight: 4
---

Let's create the main dashboard layout with a data table and charts.

## Step 1: Set Up the Layout

1. Create the section:

   - Add a new section (a column component will be added automatically)
   - Before selecting/clicking the new section, adjust the section height in the properties panel to "Fill Viewport"

:::alert{header="Important" type="warning"}
After adding the new section, if you click into the new column, the option to set section height to "Fill Viewport" will become grayed out. To regain access to this setting:

- Click the grey space below the new section
  - After selecting the grey space, you should see sections listed on the right int the properties panel
- Click "Section 3" in the properties panel
- With the section now in context, adjust the section height in the properties panel to "Fill Viewport"
  :::

  ![Body Section Example](/images/gifs/add-third-section.gif)

2. Configure the column:

   - Click inside the section component so the column is in context and set the Layout property to "Horizontal"

   ![Horizontal Layout](/images/horizontal-layout.png)

## Step 2: Add Containers

1. Left container (for table):

   - Add a container component
   - Width will be "Fluid" (¾ of section)
   - Set layout to "Vertical"
   - Set height to "Fill Parent"

2. Right container (for charts):

   - Add a container component by right clicking and selecting container
   - Set width to "Fill Parent"
   - Set layout to "Vertical"
   - Set height to "Fill Parent"

## Step 3: Add the Table

1. In the left container:

   - Add a Table component
   - Set height to "Fill Parent"

2. Simplify the table:

   - Remove the default header by going to "content" and delete the "Users" Header
   - Remove the search bar by going to "Interaction" and toggling off the search switch.

## Step 4: Add Charts

1. In the right container:

   - Add two chart components
   - Set both heights to "Fill Parent"

{{% notice tip %}}
The "Fill Parent" and "Fill Viewport" settings ensure your components use the available space effectively and create a responsive layout.
{{% /notice %}}

## Example

Here's how your body section should look after completion:

![Body Section Example](/images/gifs/updated-body-section-example.gif)

## Next Steps

Great work! Next, we'll implement the APIs to populate your application with data.
