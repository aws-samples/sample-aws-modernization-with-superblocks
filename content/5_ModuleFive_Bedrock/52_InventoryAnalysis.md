---
title: "Inventory Analysis with Bedrock"
weight: 2
---

## Building an Inventory Analysis Feature 📦

Now that we have our Bedrock connection set up, let's create a powerful inventory analysis feature that can provide insights about current inventory levels, identify potential stockouts, and recommend reordering strategies.

### Step 1: Create the Inventory Analysis API 🔍

1. Navigate to **API Builder** in the left sidebar
2. Click **+ Create API** and select **HTTP API**
3. Name your API "InventoryAnalysis"
4. Add a new step and select **Data Source Query**
5. Configure the step:
   - Select your database resource
   - Enter the following SQL query to get inventory data:

```sql
SELECT 
  product_name,
  category,
  current_stock,
  reorder_point,
  lead_time_days,
  avg_daily_sales,
  last_restock_date
FROM inventory
ORDER BY (current_stock - reorder_point) ASC
LIMIT 50;
```

6. Name this step "GetInventoryData"
7. Add another step and select **AWS Bedrock**
8. Configure the Bedrock step:
   - Select your Bedrock resource
   - Choose "Text" as the input type
   - For the prompt, we'll use a template that includes our inventory data:

```
You are an inventory management expert. Analyze the following inventory data and provide:
1. A summary of the current inventory status
2. Identification of any products at risk of stockout in the next 14 days
3. Specific reordering recommendations with quantities
4. Any patterns or insights that might help optimize inventory levels

Here's the inventory data:
{{steps.GetInventoryData}}
```

9. Name this step "AnalyzeInventory"
10. Click **Save** to save your API

::alert[When crafting prompts for foundation models, be specific about the format and type of analysis you want. This helps ensure consistent, useful responses.]{header="Tip" type="info"}

### Step 2: Create a UI Component for the Analysis 🖥️

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

### Step 3: Enhance the Analysis with Visualizations 📊

To make the analysis more impactful, let's add a visualization:

1. Add a new container below the text component
2. Add a chart component (bar chart)
3. Configure the chart to visualize products at risk:
   - Data source: `{{state.inventoryAnalysis.steps.GetInventoryData.body}}`
   - X-axis: `product_name`
   - Y-axis: `current_stock`
   - Reference line: `reorder_point`
4. Style the chart to highlight products below reorder point

### Step 4: Test Your Inventory Analysis Feature ✅

1. Preview your application
2. Click the "Analyze Inventory" button
3. Wait for the analysis to complete
4. Review the insights provided by AWS Bedrock
5. Examine the visualization to identify at-risk products

::alert[You've now created a powerful inventory analysis feature that leverages AWS Bedrock to provide actionable insights. This feature demonstrates how Superblocks and AWS Bedrock can work together to transform raw data into valuable business intelligence.]{header="Success" type="success"}

![Inventory Analysis Feature](/images/inventory-analysis-feature.png)
