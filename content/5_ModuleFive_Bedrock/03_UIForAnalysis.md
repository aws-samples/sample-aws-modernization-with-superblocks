---
title: "Building the UI Component"
chapter: true
weight: 3
---

Let's create UI components to generate and display our Bedrock-powered inventory recommendations.

## Step 1: Add the Analysis Button

1. Right click next to the 'Reset Filters' button and add a button component
2. Update the Label property in the properties panel to "Generate Inventory Insights"
3. In the Properties panel under the Event handlers section:

   - Click + next to "onClick"
   - Select "Open/close modal"
   - Next to Modal, select "New Modal"

4. Add another onClick event:

   - Click + next to "onClick"
   - Select "Run API"
   - Select the "generate_insights" API

## Step 2: Configure the Modal

1. Click the "Generate Inventory Insights" button to open the modal
2. In the properties panel, adjust the modal settings:

   - Set width to "Large"
   - Update heading to "Inventory Management Insights"

3. Add a table component to display results:

   - Select the middle section of the modal, and set the Layout property to "Vertical"
   - Click 'Add component' and add a table component
   - Select the table component and remove the default header by going to "content" and delete "Users"
   - Remove the search bar by going to "Interaction" and toggling off the search switch
   - Clear the placeholder data in the "Data" field
   - Add:

   ```sh
    {{generate_insights.response.recommendations}}
   ```

## Step 3: Test the Feature

1. Preview your application
2. Click the "Generate Inventory Insights" button
3. Wait for the analysis to complete
4. Review the insights provided by Amazon Bedrock
5. Examine the visualization to identify at-risk products

## Example

Here's how your analysis component should look:
<br>
<img src="/images/inventory-analysis-feature.png" width="700" height="350" />

:::alert{type="success"}
Congratulations! You've created a powerful inventory analysis feature that leverages Amazon Bedrock to provide actionable insights. This feature demonstrates how Superblocks and Amazon Bedrock can work together to transform raw data into valuable business intelligence.
:::
