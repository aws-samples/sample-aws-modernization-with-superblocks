---
title: "Natural Language Queries"
weight: 4
---

## Building a Natural Language Query Interface 💬

<!-- One of the most powerful capabilities of AWS Bedrock is its ability to understand and process natural language. In this section, we'll create a natural language query interface that allows users to ask questions about their sales data in plain English.

### Step 1: Create the Natural Language Query API 🔍

1. Navigate to **API Builder** in the left sidebar
2. Click **+ Create API** and select **HTTP API**
3. Name your API "NaturalLanguageQuery"
4. Add a parameter called "userQuery" of type "string"
5. Add a step and select **AWS Bedrock**
6. Configure the step:
   - Select your Bedrock resource
   - Choose "Text" as the input type
   - For the prompt, we'll create a system that translates natural language to SQL:

```
You are an expert SQL translator. Your job is to convert natural language questions about sales data into SQL queries.

The database has the following schema:
- sales(id, date, product_id, customer_id, quantity, unit_price, total_price)
- products(id, name, category, supplier_id, cost_price)
- customers(id, name, region, segment)

Convert the following question into a SQL query:
{{params.userQuery}}

Return ONLY the SQL query without any explanation or additional text.
```

7. Name this step "TranslateToSQL"
8. Add another step and select **Data Source Query**
9. Configure the step:
   - Select your database resource
   - For the query, use the output from the previous step:
   - Set the query to `{{steps.TranslateToSQL.body}}`
10. Name this step "ExecuteQuery"
11. Add a final step using **AWS Bedrock**
12. Configure the step:
   - Select your Bedrock resource
   - Choose "Text" as the input type
   - For the prompt, we'll ask Bedrock to explain the results:

```
You are a sales data analyst. Explain the following query results in a clear, concise way that highlights the most important insights. The original question was: "{{params.userQuery}}"

Here are the query results:
{{steps.ExecuteQuery.body}}

Provide your explanation in markdown format with appropriate headings and bullet points.
```

13. Name this step "ExplainResults"
14. Click **Save** to save your API

::alert[This multi-step approach demonstrates the power of chaining AI capabilities: first translating natural language to SQL, then executing the query, and finally explaining the results in human-friendly terms.]{header="Note"}

### Step 2: Create the Natural Language Interface 🖥️

Now, let's create a user interface for the natural language query feature:

1. Navigate to **UI Builder** in the left sidebar
2. Open your dashboard page or create a new page
3. Add a new container with a heading "Ask Questions About Your Sales Data"
4. Add a text input component with placeholder text "e.g., What were our top 5 selling products last month?"
5. Add a button labeled "Ask"
6. Configure the button's onClick event:
   - Select "Run API"
   - Choose your "NaturalLanguageQuery" API
   - Set the userQuery parameter to the text input value
   - For the success action, select "Update State"
   - Set the state key to "queryResults"
7. Add a container below to display results
8. Add a text component to show the SQL query:
   - Set the content to `SQL Query: {{state.queryResults.steps.TranslateToSQL.body}}`
   - Style as code block
9. Add a data table component to show the raw results:
   - Set the data source to `{{state.queryResults.steps.ExecuteQuery.body}}`
10. Add a text component to show the explanation:
    - Set the content to `{{state.queryResults.steps.ExplainResults.body}}`
    - Enable markdown rendering
11. Add appropriate loading states and error handling

::alert[Consider adding error handling for cases where the AI generates invalid SQL. You can add a try/catch in a JavaScript step to validate the SQL before executing it.]{header="Tip" type="info"}

### Step 3: Test Your Natural Language Query Interface ✅

1. Preview your application
2. Enter a question like "What were our top 5 selling products last month?"
3. Click "Ask"
4. Observe how the system:
   - Translates your question to SQL
   - Executes the query against your database
   - Provides a human-readable explanation of the results

### Step 4: Enhance with Follow-up Questions 🔄

To make the interface more conversational, let's add support for follow-up questions:

1. Add a state variable to store conversation history
2. Modify your API to include previous questions and answers
3. Add a "Follow-up" button that appears after the first question
4. Update the prompt to include context from previous interactions

::alert[This natural language query interface demonstrates how AWS Bedrock and Superblocks can work together to create intuitive, powerful data exploration tools that don't require technical expertise to use.]{header="Success" type="success"}

![Natural Language Query Interface](/images/nl-query-interface.png) -->
