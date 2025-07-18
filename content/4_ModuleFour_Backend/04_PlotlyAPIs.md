---
title: "Creating Plotly Visualization APIs"
chapter: true
weight: 4
---

Let's create two interactive Plotly charts: a location-based sales chart and a monthly trends visualization.

## Step 1: Create Location Sales Chart

1. Open API Builder:

   - Press CMD/CTRL + U

2. Create the API:

   - Click "Add new API"
   - Search for and select your database integration (aws-superblocks-rds)
   - Click the pen icon next to 'API1' and rename it to "location_chart"

3. Add the below SQL query to the Postgres step:

   ```sql
   SELECT location_name, SUM(total_amount) as total_sales
   FROM dm_operations.sales
   GROUP BY location_name
   ORDER BY total_sales DESC;
   ```

4. Click the plus sign below the Postgres step, add a Python Function Step, and add the below code:

   ```python
   import plotly.express as px
   import pandas as pd

   # Create bar chart
   fig = px.bar(
       Step1.output,
       x="location_name",
       y="total_sales",
       title="Sales by Location",
       labels={
           "total_sales": "Total Sales ($)",
           "location_name": "Branch Location"
       },
       color="location_name"
   )

   # Style the chart
   fig.update_layout(
       template="plotly_white",
       showlegend=False,
       height=400
   )

   return fig.to_json()
   ```

## Step 2: Create Monthly Trends Chart

1. Create the API:

   - From the API builder, click "Add new API"
   - Search for and select your database integration (aws-superblocks-rds)
   - Click the pen icon next to 'API1' and rename it to "monthly_trends"

2. Add the below SQL query to the Postgres step:

   ```sql
   SELECT
       DATE_TRUNC('month', sale_date) as month,
       SUM(total_amount) as total_sales,
       COUNT(*) as number_of_orders
   FROM dm_operations.sales
   WHERE sale_date >= CURRENT_DATE - INTERVAL '12 months'
   GROUP BY DATE_TRUNC('month', sale_date)
   ORDER BY month;
   ```

3. Click the plus sign below the Postgres step, add a Python Function Step, and add the below code:

   ```python
   import plotly.graph_objects as go
   import pandas as pd

   # Prepare data
   monthly_data = pd.DataFrame(Step1.output)
   monthly_data["total_sales"] = pd.to_numeric(monthly_data["total_sales"])
   monthly_data["number_of_orders"] = pd.to_numeric(monthly_data["number_of_orders"])

   # Create figure
   fig = go.Figure()

   # Add sales trend
   fig.add_trace(
       go.Scatter(
           x=monthly_data["month"],
           y=monthly_data["total_sales"],
           name="Total Sales",
           line=dict(color="blue", width=2)
       )
   )

   # Add orders trend
   fig.add_trace(
       go.Scatter(
           x=monthly_data["month"],
           y=monthly_data["number_of_orders"],
           name="Number of Orders",
           yaxis="y2",
           line=dict(color="red", width=2)
       )
   )

   # Style the chart
   fig.update_layout(
       title="Monthly Sales Trends (Last 12 Months)",
       xaxis=dict(title="Month"),
       yaxis=dict(
           title="Total Sales ($)",
           titlefont=dict(color="blue"),
           tickfont=dict(color="blue")
       ),
       yaxis2=dict(
           title="Number of Orders",
           titlefont=dict(color="red"),
           tickfont=dict(color="red"),
           anchor="x",
           overlaying="y",
           side="right"
       ),
       template="plotly_white",
       height=400
   )

   return fig.to_json()

   ```

## Step 3: Connect Your Charts

1. Set up location chart:

   - Select the first chart component
   - Clear the text in the "Header" property
   - Set "Definition" to "Plotly"
   - Set "Plotly chart JSON" to:

   ```sh
   {{location_chart.response}}
   ```

2. Set up trends chart:

   - Select the second chart component
   - Clear the text in the "Header" property
   - Set "Definition" to "Plotly"
   - Set "Plotly chart JSON" to:

   ```sh
   {{monthly_trends.response}}
   ```

{{% notice tip %}}
Your charts are interactive! Users can hover over points, zoom, and pan. If charts don't appear, try clicking "Run API" in the API Builder Tool.
{{% /notice %}}

## Testing

1. Test rendering:

   - Check both charts appear
   - Verify data accuracy
   - Test hover tooltips

2. Test interactions:

   - Try zooming in/out
   - Pan across the data
   - Click legend items

## Next Steps

Now that your dashboard is working, let's integrate with Amazon Bedrock!
