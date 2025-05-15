---
title: "Creating Plotly Visualization APIs"
chapter: true
weight: 4
---

# Creating Plotly Visualization APIs

In this section, we'll create two APIs that generate Plotly charts for our dashboard: a location-based sales chart and a monthly trends chart.

## Location Sales Chart API

1. Open the API Builder Tool (CMD/CTRL + U)
2. Click "Add new API"
3. Search for the database integration
4. Click the pencil icon next to 'API1' and rename the API to "location_chart"

Add the below SQL to the database integration:

```sql
SELECT location_name, SUM(total_amount) as total_sales
FROM dm_operations.sales
GROUP BY location_name
ORDER BY total_sales DESC;
```

Add a Python Function Step and add the below code:

```python
import plotly.express as px
import pandas as pd

fig = px.bar(
    Step1.output,
    x="location_name",
    y="total_sales",
    title="Sales by Location",
    labels={"total_sales": "Total Sales ($)", "location_name": "Branch Location"},
    color="location_name",
)

fig.update_layout(
    template="plotly_white",
    showlegend=False,
    height=400
)

return fig.to_json()
```

## Monthly Trends Chart API

1. Open the API Builder Tool (CMD/CTRL + U)
2. Click "Add new API"
3. Search for the Postgres integration
4. Click the pencil icon next to 'API1' and rename the API to "monthly_trends"

Add the below SQL to the database integration:

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

3. Add a Python Function Step:

```python
import plotly.graph_objects as go
import pandas as pd

monthly_data = pd.DataFrame(Step1.output)
monthly_data["total_sales"] = pd.to_numeric(monthly_data["total_sales"])
monthly_data["number_of_orders"] = pd.to_numeric(monthly_data["number_of_orders"])

fig = go.Figure()

# Add sales line
fig.add_trace(
    go.Scatter(
        x=monthly_data["month"],
        y=monthly_data["total_sales"],
        name="Total Sales",
        line=dict(color="blue", width=2),
    )
)

# Add orders line
fig.add_trace(
    go.Scatter(
        x=monthly_data["month"],
        y=monthly_data["number_of_orders"],
        name="Number of Orders",
        yaxis="y2",
        line=dict(color="red", width=2),
    )
)

# Update layout
fig.update_layout(
    title="Monthly Sales Trends (Last 12 Months)",
    xaxis=dict(title="Month"),
    yaxis=dict(
        title="Total Sales ($)",
        titlefont=dict(color="blue"),
        tickfont=dict(color="blue"),
    ),
    yaxis2=dict(
        title="Number of Orders",
        titlefont=dict(color="red"),
        tickfont=dict(color="red"),
        anchor="x",
        overlaying="y",
        side="right",
    ),
    template="plotly_white",
    height=400,
)

return fig.to_json()
```

## Connect the Chart Components

1. Configure Top Chart (Location Sales):

   - Select the first chart component
   - Clear the "Header" value
   - Set "Definition" to "Plotly"
   - Set "Plotly chart JSON" to: {{location_chart.response}}

2. Configure Bottom Chart (Monthly Trends):
   - Select the second chart component
   - Clear the "Header" value
   - Set "Definition" to "Plotly"
   - Set "Plotly chart JSON" to: {{monthly_trends.response}}

{{% notice tip %}}
Plotly charts are interactive by default. Users can hover over data points, zoom, and pan the charts. If data is not populating in the table, try clicking the "Run API" button again in the API Builder Tool.
{{% /notice %}}

## Testing

1. Verify both charts render correctly
2. Test interactivity features

## Next Steps

Now that we've completed all the backend APIs, we'll move on to integrating with AWS Bedrock.
