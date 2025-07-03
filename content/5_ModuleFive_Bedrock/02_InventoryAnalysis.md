---
title: "Inventory Analysis with Bedrock"
weight: 2
---

## Building an Inventory Analysis Feature 📦

Now that we have our Bedrock connection set up, let's create a powerful inventory analysis feature that can provide insights about current inventory levels and recommend transferring strategies.

### Step 1: Create the Inventory Analysis API

1. Create a new API:

   - Open the API Builder Tool (CMD/CTRL + U)
   - Click "Add new API"
   - Select your database integration (aws-superblocks-rds)
   - Click the pencil icon next to API1 and rename it to "generate_insights"

2. Configure the data source:

   - Rename the postgres step to "get_input_data"
   - Add the below SQL query to the Postgres step:

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

Postgres SQL Step:
<img src="/images/generate-insights.png" width="700" height="350" />

### Step 2: Process the Data

1. Add data preprocessing:

   - Add a new Python Function step after the SQL query by clicking the "+" under the get_input_data block
   - Name the step "simplify_input_data"
   - Add the below Python code to prepare data for the AI model:

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

### Step 3: Connect to Amazon Bedrock

1. **Create the Bedrock Python Function:**

   - Add a new Python Function step after the "simplify_input_data" step
   - Name the step "send_to_bedrock"
   - Add the below Python code to send data to the AI model:

```python
import boto3
import json

def send_to_bedrock(text_data):
    # Truncate data to stay within token limits
    text_data = text_data[:5000]

    # Access credentials from Superblocks organization secrets
    creds = json.loads(sb_secrets.myuseraccesskey.MyUserAccessKey)

    # Create Bedrock client using retrieved credentials
    client = boto3.client(
        service_name="bedrock-runtime",
        region_name="us-east-1",  # Use the region where you enabled Bedrock models
        aws_access_key_id=creds["AccessKeyId"],
        aws_secret_access_key=creds["SecretAccessKey"]
    )

    # Create a focused prompt for inventory transfer recommendations
    prompt = f"""Based on this inventory data: {text_data}

Provide 5 inventory transfer recommendations as bullet points. For each recommendation, include:
• Product name and quantity to transfer
• From location → To location
• Brief reasoning

Format as JSON with this structure:
[
  {{
    "product": "Product Name",
    "quantity": 10,
    "from_location": "Location A",
    "to_location": "Location B",
    "reasoning": "Brief explanation"
  }}
]"""

    try:
        # Make request to Amazon Bedrock
        response = client.invoke_model(
            modelId="us.amazon.nova-micro-v1:0",  # Using Amazon Nova Micro for cost efficiency
            body=json.dumps({
                "messages": [
                    {
                        "role": "user",
                        "content": [{"text": prompt}]
                    }
                ]
            }),
            contentType="application/json",
            accept="application/json"
        )

        # Parse the response
        response_body = json.loads(response["body"].read())

        if "content" in response_body and isinstance(response_body["content"], list):
            output = response_body["content"][0]["text"]
        else:
            output = str(response_body)

        return {"raw_output": output}

    except Exception as e:
        return {"error": f"Bedrock API call failed: {str(e)}"}

# Call the function with the processed data
return send_to_bedrock(simplify_input_data.output)
```

## 🔍 Code Breakdown: Understanding the Bedrock Integration

Let's break down the key components of this integration to help you understand and customize it:

### 🔐 **AWS Secrets Manager Integration**

```python
# Access credentials from Superblocks organization secrets
creds = json.loads(sb_secrets.myuseraccesskey.MyUserAccessKey)
```

**How it works:**

- **`sb_secrets`**: Superblocks' built-in object for accessing Organization-level secrets
- **`myuseraccesskey`**: The name of your secret store configured in Organization Settings
- **`MyUserAccessKey`**: The actual AWS secret name in your AWS Secrets Manager
- **`json.loads()`**: Parses the JSON string returned from AWS Secrets Manager

**The Flow:**

1. Superblocks → Your AWS Account → AWS Secrets Manager → Retrieves JSON: `{"AccessKeyId": "AKIA...", "SecretAccessKey": "..."}`
2. Parse JSON → Extract individual credentials → Use with boto3

---

### 🤖 **Amazon Bedrock Model Selection**

```python
modelId="us.amazon.nova-micro-v1:0"  # Using Amazon Nova Micro for cost efficiency
```

**Available Models** (you can replace the `modelId` with any of these):

| **Model Family**     | **Model ID**                                | **Best For**                | **Cost**           |
| -------------------- | ------------------------------------------- | --------------------------- | ------------------ |
| **Amazon Nova**      | `us.amazon.nova-micro-v1:0`                 | Quick tasks, cost-effective | 💰 Lowest          |
|                      | `us.amazon.nova-lite-v1:0`                  | Balanced performance        | 💰💰 Low           |
|                      | `us.amazon.nova-pro-v1:0`                   | Complex reasoning           | 💰💰💰 Medium      |
|                      | `us.amazon.nova-premier-v1:0`               | Most capable tasks          | 💰💰💰💰 Higher    |
| **Anthropic Claude** | `anthropic.claude-3-5-sonnet-20241022-v2:0` | Advanced reasoning          | 💰💰💰 Medium-High |
|                      | `anthropic.claude-3-haiku-20240307-v1:0`    | Fast responses              | 💰💰 Low-Medium    |

**💡 Pro Tip:** You can find all available model IDs in the [Amazon Bedrock Console](https://console.aws.amazon.com/bedrock/home#/model-access) under "Model access"

---

### 🌍 **Region Configuration**

```python
region_name="us-east-1"  # Use the region where you enabled Bedrock models
```

**Important Notes:**

- **Bedrock Models**: Available in specific regions (us-east-1, us-west-2, eu-west-1, etc.)
- **Your AWS Secret**: Located in `us-west-2` (but accessed through Superblocks)
- **Cross-Region Access**: Superblocks handles the cross-region secret access automatically

---

### 💬 **Prompt Engineering**

```python
prompt = f"""Based on this inventory data: {text_data}

Provide 5 inventory transfer recommendations as bullet points...

Format as JSON with this structure:
[
  {{
    "product": "Product Name",
    "quantity": 10,
    "from_location": "Location A",
    "to_location": "Location B",
    "reasoning": "Brief explanation"
  }}
]"""
```

**Customization Options:**

- **Change the number**: Replace `5` with any number of recommendations
- **Modify the format**: Request different output formats (CSV, XML, plain text)
- **Add constraints**: Include budget limits, distance restrictions, etc.
- **Change the task**: Modify for demand forecasting, stock optimization, etc.

---

### 🔧 **Easy Customizations**

**1. Switch to a More Powerful Model:**

```python
modelId="us.amazon.nova-pro-v1:0"  # More capable reasoning
```

**2. Change the Analysis Type:**

```python
prompt = f"""Based on this inventory data: {text_data}

Analyze demand patterns and provide 3 restocking recommendations...
```

**3. Modify Output Format:**

```python
prompt = f"""...Format as a simple bullet list with no JSON."""
```

**4. Add Business Context:**

```python
prompt = f"""You are a supply chain expert. Based on this inventory data: {text_data}
Consider seasonal trends and provide recommendations...
```

:::alert{header="🎯 Model Selection Guide" type="info"}

- **Development/Testing**: Use `nova-micro` for cost efficiency
- **Production/Complex Analysis**: Use `nova-pro` or `claude-3-5-sonnet`
- **Quick Responses**: Use `nova-lite` or `claude-haiku`
- **Most Advanced Tasks**: Use `nova-premier` for maximum capability
  :::

:::alert{header="Important" type="info"}
This approach uses Superblocks Organization-level secrets:

- **Direct Access**: Uses `sb_secrets.myuseraccesskey.MyUserAccessKey` to access the secret directly (no string interpolation needed in Python functions)
- **JSON Parsing**: The secret is returned as a JSON string, so we parse it with `json.loads()`
- **Credential Extraction**: Extract individual credentials using dictionary access: `creds["AccessKeyId"]`

This follows the correct Superblocks pattern for accessing AWS Secrets Manager in Python functions.
:::

:::alert{type="info"}
This approach uses Organization-level secrets to securely retrieve your AWS credentials, following security best practices by:

- Keeping credentials encrypted and centrally managed in Superblocks
- Avoiding hardcoded credentials in your application code
- Enabling credential rotation through Organization Settings
- Restricting secret access to Backend APIs only (not Frontend components)
  :::

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

:::alert{type="info"}
When crafting prompts for foundation models:

- Be specific about the format you want
- Include examples when possible
- Define clear constraints
- Test and refine your prompts
  :::

:::alert{type="success"}
Congratulations! You've created a powerful inventory analysis feature that:

- Analyzes inventory data in real-time
- Provides actionable transfer recommendations
- Helps optimize stock levels across locations
  :::

## Working with AWS Services using boto3

The boto3 library is AWS's SDK for Python, providing a powerful interface to interact with any AWS service programmatically. Just as we used boto3 to connect with Amazon Bedrock above, you can use the same pattern to interact with other AWS services like Amazon Simple Storage Service (Amazon S3), Amazon DynamoDB, or AWS Lambda, making it easy to build comprehensive AWS applications.

## Next Steps

In the next section, we'll add a UI component to display the recommendations generated from Amazon Bedrock. 🚀
