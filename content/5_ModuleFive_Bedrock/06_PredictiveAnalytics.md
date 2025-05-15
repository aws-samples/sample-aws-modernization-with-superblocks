---
title: "Predictive Analytics"
weight: 6
---

## Implementing Predictive Analytics for Inventory 🔮

<!-- In this section, we'll use AWS Bedrock to implement predictive analytics for inventory management. This feature will help businesses anticipate future inventory needs based on historical data and trends.

### Step 1: Create the Prediction API 📈

1. Navigate to **API Builder** in the left sidebar
2. Click **+ Create API** and select **HTTP API**
3. Name your API "InventoryPrediction"
4. Add a parameter called "productId" of type "string"
5. Add a parameter called "predictionDays" of type "number" with a default value of 30
6. Add a step and select **Data Source Query**
7. Configure the step:
   - Select your database resource
   - Enter the following SQL query to get historical sales data:

```sql
SELECT
  date,
  SUM(quantity) as daily_sales
FROM sales
WHERE product_id = {{params.productId}}
GROUP BY date
ORDER BY date DESC
LIMIT 90;
```

8. Name this step "GetHistoricalSales"
9. Add another step and select **Data Source Query**
10. Configure the step:
    - Select your database resource
    - Enter the following SQL query to get product information:

```sql
SELECT
  name,
  category,
  current_stock,
  reorder_point,
  lead_time_days
FROM inventory
JOIN products ON inventory.product_id = products.id
WHERE product_id = {{params.productId}};
```

11. Name this step "GetProductInfo"
12. Add a step using **AWS Bedrock**
13. Configure the step:
    - Select your Bedrock resource
    - Choose "Text" as the input type
    - For the prompt, we'll ask Bedrock to predict future inventory needs:

```
You are an inventory forecasting expert. Based on the following historical sales data and product information, predict the daily sales for the next {{params.predictionDays}} days and determine if and when a reorder should be placed.

Historical Sales Data (last 90 days):
{{steps.GetHistoricalSales.body}}

Product Information:
{{steps.GetProductInfo.body}}

Provide your analysis in the following JSON format:
{
  "product_name": "Product Name",
  "daily_sales_prediction": [
    {"day": 1, "predicted_sales": X},
    {"day": 2, "predicted_sales": X},
    ...
  ],
  "total_predicted_sales": X,
  "days_until_stockout": X,
  "reorder_recommendation": {
    "should_reorder": true/false,
    "recommended_order_quantity": X,
    "recommended_order_date": "YYYY-MM-DD"
  },
  "confidence_level": "high/medium/low",
  "reasoning": "Brief explanation of the prediction logic"
}
```

14. Name this step "GeneratePrediction"
15. Add a final step using **JavaScript**
16. Configure the step:
    - Enter the following code to parse the JSON response:

```javascript
// Parse the prediction response
const predictionText = steps.GeneratePrediction.body;
let prediction;

try {
  // Extract JSON from the response (in case the model included extra text)
  const jsonMatch = predictionText.match(/\{[\s\S]*\}/);
  if (jsonMatch) {
    prediction = JSON.parse(jsonMatch[0]);
  } else {
    throw new Error("No JSON found in response");
  }
} catch (error) {
  return {
    error: "Failed to parse prediction",
    details: error.message,
    rawResponse: predictionText
  };
}

// Calculate additional metrics
const currentStock = steps.GetProductInfo.body[0].current_stock;
const dailyPredictions = prediction.daily_sales_prediction;

// Generate data for chart
const chartData = dailyPredictions.map(day => ({
  day: day.day,
  predicted_sales: day.predicted_sales,
  remaining_stock: Math.max(0, currentStock - dailyPredictions
    .filter(d => d.day <= day.day)
    .reduce((sum, d) => sum + d.predicted_sales, 0))
}));

return {
  prediction,
  chartData,
  productInfo: steps.GetProductInfo.body[0]
};
```

17. Name this step "FormatResults"
18. Click **Save** to save your API

::alert[Foundation models like Claude can generate structured data like JSON, but they sometimes include additional text. The JavaScript step ensures we extract and parse only the JSON portion of the response.]{header="Info"}

### Step 2: Create the Prediction UI 🖥️

Now, let's create a user interface for the prediction feature:

1. Navigate to **UI Builder** in the left sidebar
2. Open your dashboard page or create a new page
3. Add a new container with a heading "Inventory Prediction"
4. Add a dropdown component to select a product
   - Populate it with a list of products from your database
5. Add a number input for "Prediction Days" with default value 30
6. Add a button labeled "Generate Prediction"
7. Configure the button's onClick event:
   - Select "Run API"
   - Choose your "InventoryPrediction" API
   - Set the parameters from the dropdown and input
   - For the success action, select "Update State"
   - Set the state key to "predictionResults"
8. Add a container to display the prediction results
9. Add a text component to show the summary:
   - Set the content to display key prediction metrics
   - Use conditional formatting to highlight critical information
10. Add a line chart component:
    - Set the data source to `{{state.predictionResults.steps.FormatResults.body.chartData}}`
    - Configure series for "predicted_sales" and "remaining_stock"
    - Add a reference line for the reorder point
11. Add a card component to show the reorder recommendation:
    - Display the recommended order quantity and date
    - Add a visual indicator of urgency based on days until stockout
12. Add appropriate loading states and error handling

::alert[Use color coding to make the prediction results more intuitive. For example, use red for products that need immediate reordering, yellow for those approaching reorder point, and green for those with sufficient stock.]{header="Tip" type="info"}

### Step 3: Test Your Prediction Feature ✅

1. Preview your application
2. Select a product from the dropdown
3. Click "Generate Prediction"
4. Review the prediction results, including:
   - Daily sales forecast
   - Projected stockout date
   - Reorder recommendations
   - Visualization of inventory levels over time

::alert[This predictive analytics feature demonstrates how AWS Bedrock can be used to create sophisticated forecasting tools that help businesses make data-driven inventory decisions.]{header="Success" type="success"}

![Inventory Prediction Feature](/images/inventory-prediction-feature.png) -->
