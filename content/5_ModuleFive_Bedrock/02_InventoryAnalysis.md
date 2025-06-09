---
title: "Inventory Analysis with Bedrock"
weight: 2
---

## Building an Inventory Analysis Feature 📦

Now that we have our Bedrock connection set up, let's create a powerful inventory analysis feature that can provide insights about current inventory levels and recommend transferring strategies.

### Step 1: Create the Inventory Analysis API

1. Create a new API:
   - Open the API Builder Tool (CMD/CTRL + U)
   - Click the pencil icon next to API1 and rename it to "generate_insights"

2. Configure the data source:
   - Search for your database integration
   - Add a new SQL step and rename it to "get_input_data"
   - Add the query below:

```sql
SELECT 
ils.inventory_id, ils.sku, ils.product_name, ils.category_name,
ils.location_name, ils.current_stock, ils.reorder_point, 
ils.stock_margin, ils.stock_status,
sv.daily_velocity, po.total_quantity_ordered
FROM dm_operations.inventory_location_status ils
LEFT JOIN dm_operations.sales_velocity sv ON ils.inventory_id = sv.inventory_id AND ils.location_name = sv.location_name
LEFT JOIN dm_operations.pending_orders po ON ils.inventory_id = po.inventory_id AND ils.location_name = po.location_name;
```

### Step 2: Process the Data

1. Add data preprocessing:
   - Add a new Python step after the SQL query
   - Name the step "simplify_input_data"
   - Add the code below to prepare data for the AI model:

```python
def prepare_data_for_llm(input_data):
    import json

    # Convert JSON object to a formatted string representation
    if isinstance(input_data, (dict, list)):

        # Convert to a nicely formatted string with indentation
        formatted_string = json.dumps(input_data, indent=2)

        # Truncate if too large
        max_chars = 8000
        if len(formatted_string) > max_chars:
            formatted_string = formatted_string[:max_chars] + "\n...(truncated)"

        return formatted_string

return prepare_data_for_llm(get_input_data.output)
```

### Step 3: Connect to AWS Bedrock

1. Create the Bedrock integration:
   - Add a new Python step
   - Name it "send_to_bedrock"
   - Add the following code to interact with AWS Bedrock
   - Use the AWS integration you created in the previous section

```python
import boto3
import json


def send_to_bedrock(text_data):
    # Truncate data
    text_data = text_data[:5000]

    # Create client using the AWS integration you set up
    client = boto3.client(
        service_name="bedrock-runtime",
        region_name="us-east-1",  # Use the region where you enabled Bedrock models
        aws_access_key_id=env.BEDROCK_ACCESS_KEY_ID,  # Use the environment variable
        aws_secret_access_key=env.BEDROCK_SECRET_ACCESS_KEY,  # Use the environment variable
    )

    # Simple prompt for bullet points
    prompt = f"""Based on this inventory data: {text_data}

Provide 5 inventory transfer recommendations as bullet points. For each recommendation, include:
• Product name and quantity to transfer
• From location → To location  
• Brief reasoning

Format as JSON."""

    # Make request
    response = client.invoke_model(
        modelId="us.amazon.nova-micro-v1:0",
        body=json.dumps(
            {"messages": [{"role": "user", "content": [{"text": prompt}]}]}
        ),
        contentType="application/json",
        accept="application/json",
    )

    # Parse response
    response_body = json.loads(response["body"].read())

    if "content" in response_body and isinstance(response_body["content"], list):
        output = response_body["content"][0]["text"]
    else:
        output = str(response_body)

    return {"raw_output": output}


# Call the function
return send_to_bedrock(simplify_input_data.output)
```

{{% notice tip %}}
If you need to check your Bedrock credentials again, you can run this command in your terminal:
```bash
echo "AWS Access Key ID: $BEDROCK_ACCESS_KEY_ID"
echo "AWS Secret Access Key: $BEDROCK_SECRET_ACCESS_KEY"
```
{{% /notice %}}

### Step 4: Format the Results

1. Process the AI response:
   - Add a final Python step
   - Name it "format_output"
   - Add the code below to parse and structure the recommendations as JSON:

````python
import json
import ast
import re

def parse_bullet_recommendations(raw_output):
    try:
        # Extract text from various input formats
        text = (
            raw_output.get("raw_output", str(raw_output))
            if isinstance(raw_output, dict)
            else str(raw_output)
        )

        # Handle nested structure from Bedrock
        if text.startswith("{'"):
            data = ast.literal_eval(text)
            content_text = (
                data.get("output", {})
                .get("message", {})
                .get("content", [{}])[0]
                .get("text", "")
            )

            # Clean up escaped characters
            content_text = (
                content_text.replace("\\n", "\n")
                .replace('\\"', '"')
                .replace("\\'", "'")
            )
        else:
            content_text = text

        # Extract JSON from code block
        json_match = re.search(r"```json\s*\n(.*?)\n\s*```", content_text, re.DOTALL)
        if json_match:
            json_text = json_match.group(1)
            recommendations = json.loads(json_text)
            return {"recommendations": recommendations, "count": len(recommendations)}

        # If no JSON code block found, return error
        return {"error": "No JSON code block found", "original_output": content_text}

    except Exception as e:
        return {"error": str(e), "original_output": raw_output}


# Call the function
raw_output = send_to_bedrock.output
return parse_bullet_recommendations(raw_output)
````

### Step 5: Test the Integration

1. Test your new API:
   - Click **Run API** to execute the workflow
   - Review the recommendations in the response
   - Verify the data formatting and structure
2. Click the "Runs on" button (underneath the API name) and update the "Run on page load" value to "Never"
   - This will prevent the API from running automatically when the page loads as we will be running it whenever a user clicks a button component

{{% notice tip %}}
When crafting prompts for foundation models:
- Be specific about the format you want
- Include examples when possible
- Define clear constraints
- Test and refine your prompts
{{% /notice %}}

{{% notice success %}}
Congratulations! You've created a powerful inventory analysis feature that:
- Analyzes inventory data in real-time
- Provides actionable transfer recommendations
- Helps optimize stock levels across locations
{{% /notice %}}

## Working with AWS Services using boto3

The boto3 library is AWS's SDK for Python, providing a powerful interface to interact with any AWS service programmatically. Just as we used boto3 to connect with Bedrock above, you can use the same pattern to interact with other AWS services like S3, DynamoDB, or Lambda, making it easy to build comprehensive AWS applications.

## Next Steps

In the next section, we'll add a UI component to display the recommendations generated from Bedrock. 🚀
