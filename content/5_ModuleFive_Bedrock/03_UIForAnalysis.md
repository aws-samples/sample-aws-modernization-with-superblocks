---
title: "Building the UI Component"
chapter: true
weight: 3
---

Let's create a UI component to display our Bedrock-powered inventory recommendations.

## Step 1: Add the Analysis Button

1. Add a button component next to the "Reset Filters" button and label it "Generate Inventory Insights"
2. Adjust the width to "Fit Content"
3. Configure the button's onClick event:

   - Select "Open/close modal"
   - Next to Modal, select "New Modal"

4. Add another onClick event:

   - Select "Run API"
   - Choose your "generate_insights" API


## Step 2: Configure the Modal

1. Click the "Generate Inventory Insights" button to open the modal
2. Adjust the modal settings:

   - Set width to "Large"
   - Update heading to "Inventory Management Insights"

3. Add a table component to display results:

   - Remove placeholder data
   - Add this binding: `{{generate_insights.response.recommendations}}`


## Step 3: Test the Feature

1. Preview your application
2. Click the "Generate Inventory Insights" button
3. Wait for the analysis to complete
4. Review the insights provided by AWS Bedrock
5. Examine the visualization to identify at-risk products

## Example

Here's how your analysis component should look:
<br>
<img src="/images/inventory-analysis-feature.png" width="700" height="350" />

{{% notice success %}}
Congratulations! You've created a powerful inventory analysis feature that leverages AWS Bedrock to provide actionable insights. This feature demonstrates how Superblocks and AWS Bedrock can work together to transform raw data into valuable business intelligence.
{{% /notice %}}
