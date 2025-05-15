---
title: "Inventory Analysis with Bedrock"
weight: 2
---

## Building an Inventory Analysis Feature 📦

Now that we have our Bedrock connection set up, let's create a powerful inventory analysis feature that can provide insights about current inventory levels, identify potential stockouts, and recommend transfering strategies.

### Step 1: Create the Inventory Analysis API 🔍

1. Navigate to **API Builder** in the left sidebar
2. Open the API Builder Tool (CMD/CTRL + U)
3. Search for the integration to your database (AWS RDS if applicable)
4. Name your API "generate_insights"
5. Add the below SQL and rename the step to "get_input_data"

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

6. Underneath the first step, add a new Python step
7. Add the below Python code to limit the input data size and rename the step to "simplify_input_data"

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

# Call the function
return prepare_data_for_llm(get_input_data.output)
```

8. Add an additional Python step and the below code to call AWS Bedrock
9. Rename the step to "send_to_bedrock"

```python
import boto3
import json

def send_to_bedrock(text_data):
    # Truncate data
    text_data = text_data[:5000]

    # Create client
    client = boto3.client(
        service_name="bedrock-runtime",
        region_name=aws_region,
        aws_access_key_id=aws_access_key,
        aws_secret_access_key=aws_secret_key,
    )

    # Create prompt as a message
    prompt = f"""Analyze this inventory data: {text_data}
      Give me 3 inventory transfer recommendations across 3 different locations with:

      - Product name
      - From location
      - To location
      - Quantity
      - Priority score (0-100)
      - Cost savings
      - Reasoning
      - Analysis points (demand, cost, impact)

      Make sure to the reasoning is informative and not a generic statement. The reasoning should be different for each recommendation. Do not restate the analysis points in the response and can include
      information on forecased demand in rationale. Return as a JSON array."""

    # Make request with content as an array
    response = client.invoke_model(
        modelId="amazon.nova-lite-v1:0",
        body=json.dumps(
            {"messages": [{"role": "user", "content": [{"text": prompt}]}]}
        ),
        contentType="application/json",
        accept="application/json",
    )

    # Parse the response
    response_body = json.loads(response["body"].read())

    # Check response structure and extract accordingly
    if "content" in response_body and isinstance(response_body["content"], list):
        output = response_body["content"][0]["text"]
    else:
        # Fallback if response format is different
        output = str(response_body)

    return {"raw_output": output}


# Call the function
return send_to_bedrock(simplify_input_data.output)

```

10. Add a 4th and final Python step with the below code, and rename it to "format_output"

````python
import json
import re


def parse_text_recommendations(raw_output):
    try:
        # Get the raw output string
        if isinstance(raw_output, dict) and "raw_output" in raw_output:
            text = raw_output["raw_output"]
        else:
            text = str(raw_output)

        # Extract the JSON array from the code block using regex
        json_match = re.search(r"```json\s*\n(.*?)\n\s*```", text, re.DOTALL)
        if json_match:
            json_text = json_match.group(1)
            # Parse the extracted JSON directly
            recommendations = json.loads(json_text)
            return recommendations

        # If we can't find a code block, try to parse the nested structure
        # First convert single quotes to double quotes for proper JSON parsing
        # But be careful with nested quotes in the JSON content
        if text.startswith("{'"):
            # This is a Python dict representation, not valid JSON
            # Use ast.literal_eval which is safer than eval
            import ast

            data = ast.literal_eval(text)

            # Navigate through the nested structure
            if "output" in data and "message" in data["output"]:
                message = data["output"]["message"]
                if "content" in message and isinstance(message["content"], list):
                    content_text = message["content"][0]["text"]

                    # Extract JSON from code block
                    json_match = re.search(
                        r"```json\s*\n(.*?)\n\s*```", content_text, re.DOTALL
                    )
                    if json_match:
                        json_text = json_match.group(1)
                        recommendations = json.loads(json_text)
                        return recommendations

        # If all else fails, run the original regex pattern
        pattern = r"(\d+)\.\s+Product name:\s+(.*?)\nFrom location:\s+(.*?)\nTo location:\s+(.*?)\nQuantity:\s+(.*?)\nPriority score.*?:\s+(.*?)\nCost savings:\s+(.*?)\nReasoning:\s+(.*?)(?=\n\n\d+\.|\n\nPlease|\Z)"
        matches = re.findall(pattern, text, re.DOTALL)
        if matches:
            recommendations = []
            for match in matches:
                recommendation = {
                    "product_name": match[1].strip(),
                    "from_location": match[2].strip(),
                    "to_location": match[3].strip(),
                    "quantity": match[4].strip(),
                    "priority_score": match[5].strip(),
                    "cost_savings": match[6].strip(),
                    "reasoning": match[7].strip(),
                }
                # Try to convert numeric fields
                try:
                    recommendation["quantity"] = int(recommendation["quantity"])
                except:
                    pass
                try:
                    recommendation["priority_score"] = int(
                        recommendation["priority_score"]
                    )
                except:
                    pass

                recommendations.append(recommendation)
            return recommendations

        return {
            "error": "Could not parse recommendations from output",
            "original_output": text,
        }

    except Exception as e:
        # Custom extraction as a last resort
        try:
            # Direct extraction of the JSON array from the text
            start_idx = text.find("[\\n  {")
            end_idx = text.find("]\\n```")

            if start_idx != -1 and end_idx != -1:
                json_text = text[start_idx : end_idx + 1]
                # Replace escaped characters
                json_text = (
                    json_text.replace("\\n", "\n")
                    .replace('\\"', '"')
                    .replace("\\'", "'")
                )
                recommendations = json.loads(json_text)
                return recommendations
        except:
            pass

        return {"error": str(e), "original_output": raw_output}


# Call the function with the Bedrock output
raw_output = send_to_bedrock.output
return parse_text_recommendations(raw_output)
````

10. Click **Run API** to test your API

{{% notice tip %}}
When crafting prompts for foundation models, be specific about the format and type of analysis you want. This helps ensure consistent, useful responses.
{{% /notice %}}

{{% notice tip %}}
You've now created a powerful inventory analysis feature that leverages AWS Bedrock to provide actionable insights. This feature demonstrates how Superblocks and AWS Bedrock can work together to transform raw data into valuable business intelligence.
{{% /notice %}}

## Next Steps
