---
title: "UI Component for Analysis"
weight: 3
---

## Create a UI Component for the Analysis 🖥️

Now that we have our Bedrock connection set up, let's create a powerful inventory analysis feature that can provide insights about current inventory levels, identify potential stockouts, and recommend transfering strategies.

### Step 1: Create a UI Component for the Analysis 🖥️

Now, let's create a UI component to display the inventory analysis:

1. Navigate to **UI Builder** in the left sidebar
2. Open your dashboard page
3. Add a new container to your dashboard
4. Add a heading component with the text "Inventory Analysis"
5. Add a button component labeled "Analyze Inventory"
6. Configure the button's onClick event:
   - Select "Run API"
   - Choose your "InventoryAnalysis" API
   - For the success action, select "Update State"
   - Set the state key to "inventoryAnalysis"
7. Add a text component below the button
8. Configure the text component to display the analysis:
   - Set the content to `{{state.inventoryAnalysis.steps.AnalyzeInventory.body}}`
   - Enable markdown rendering
9. Add a loading state to improve user experience

::alert[Superblocks' state management system makes it easy to store and display API results. The state is reactive, so your UI will automatically update when the data changes.]

### Step 3: Test Your Inventory Analysis Feature ✅

1. Preview your application
2. Click the "Analyze Inventory" button
3. Wait for the analysis to complete
4. Review the insights provided by AWS Bedrock
5. Examine the visualization to identify at-risk products

::alert[You've now created a powerful inventory analysis feature that leverages AWS Bedrock to provide actionable insights. This feature demonstrates how Superblocks and AWS Bedrock can work together to transform raw data into valuable business intelligence.]{header="Success" type="success"}

![Inventory Analysis Feature](/images/inventory-analysis-feature.png)
