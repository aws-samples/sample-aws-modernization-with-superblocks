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

2. Configure the column:

    - Set layout to "Horizontal"


## Step 2: Add Containers

1. Left container (for table):

    - Add a container component
    - Width will be "Fluid" (¾ of section)
    - Set layout to "Vertical"
    - Set height to "Fill Parent"

2. Right container (for charts):

    - Add a container component
    - Set width to "Fill Parent"
    - Set layout to "Vertical"
    - Set height to "Fill Parent"


## Step 3: Add the Table

1. In the left container:

    - Add a Table component
    - Set height to "Fill Parent"

2. Simplify the table:

    - Remove the default header
    - Remove the search bar
    - Remove the download button


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
