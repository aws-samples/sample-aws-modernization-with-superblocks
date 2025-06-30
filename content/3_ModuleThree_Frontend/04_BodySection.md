---
title: "Creating the Body Section"
chapter: true
weight: 4
---

Let's create the main dashboard layout with a data table and charts.

## Step 1: Set Up the Layout

1. Create the section:

    - Add a new section (adds a column automatically)
    - Set section height to "Fill Viewport"

:::alert{header="Important" type="warning"}
After adding the new section, if you click into the new column, the option to set section height to "Fill Viewport" will become grayed out. To regain access to this setting:
- Click outside the column to deselect it (then click the column again to reselect it), OR
- Click on "Section 3" in the top right properties panel
:::

2. Configure the column:

    - Click inside the column and Set Layout to "Horizontal"

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

![Body Section Example](/images/gifs/body-section-example.gif)

## Next Steps

Great work! Next, we'll implement the APIs to populate your application with data.
