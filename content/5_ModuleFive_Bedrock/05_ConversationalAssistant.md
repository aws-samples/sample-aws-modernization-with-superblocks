---
title: "Conversational Assistant"
weight: 5
---

## Building a Conversational Business Assistant 🤖

<!-- In this final section, we'll create a conversational assistant that can help users interact with their business data through natural dialogue. This assistant will leverage AWS Bedrock's language capabilities to understand context, answer questions, and provide insights.

### Step 1: Create the Conversational Assistant API 🔍

1. Navigate to **API Builder** in the left sidebar
2. Click **+ Create API** and select **HTTP API**
3. Name your API "BusinessAssistant"
4. Add a parameter called "userMessage" of type "string"
5. Add a parameter called "conversationHistory" of type "array" (optional)
6. Add a step and select **AWS Bedrock**
7. Configure the step:
   - Select your Bedrock resource
   - Choose "Text" as the input type
   - For the prompt, we'll create a system message that defines the assistant's capabilities:

```
You are a helpful business assistant for a retail company. You can help with inventory management, sales analysis, and customer insights. You have access to the following data:

1. Inventory data: product names, categories, current stock levels, reorder points
2. Sales data: daily sales by product, region, and customer segment
3. Customer data: customer segments, purchase history, and preferences

When asked about specific data, respond as if you have access to this information and provide insights based on it. Keep your responses concise, professional, and focused on business value.

{{#if params.conversationHistory}}
Previous conversation:
{{params.conversationHistory}}
{{/if}}

User: {{params.userMessage}}
Assistant:
```

8. Name this step "GenerateResponse"
9. Click **Save** to save your API

::alert[The system message is crucial for defining the assistant's personality, capabilities, and constraints. A well-crafted system message ensures the assistant provides relevant, helpful responses.]{header="Info"}

### Step 2: Create the Conversational Interface 💬

Now, let's create a user interface for the conversational assistant:

1. Navigate to **UI Builder** in the left sidebar
2. Create a new page called "Business Assistant"
3. Design a chat-like interface:
   - Add a container that takes up most of the page
   - Style it to look like a chat window
   - Add a state variable called "messages" initialized as an empty array
4. Add a text input at the bottom of the page with a send button
5. Configure the send button's onClick event:
   - Get the current message from the text input
   - Update the "messages" state to add the user message
   - Clear the text input
   - Run the "BusinessAssistant" API with:
     - userMessage: the current message
     - conversationHistory: formatted previous messages
   - On success, update the "messages" state to add the assistant's response
6. Create a component to render each message in the chat:
   - Different styling for user vs. assistant messages
   - Support for markdown in assistant responses
   - Timestamps for each message
7. Add a "Clear Conversation" button that resets the messages state

::alert[To improve the user experience, add a typing indicator while waiting for the assistant's response. This provides visual feedback that the system is processing the request.]{header="Tip" type="info"}

### Step 3: Enhance the Assistant with Data Integration 📊

To make the assistant more useful, let's integrate it with real data:

1. Modify the BusinessAssistant API to detect when the user is asking for specific data
2. Add conditional steps that query the database when relevant
3. Include the query results in the prompt to Bedrock
4. Update the UI to display charts or tables when appropriate

For example, if the user asks "How are sales trending this month?", the API should:
1. Detect this is a sales trend question
2. Query the sales data for the current month
3. Include this data in the prompt to Bedrock
4. Generate a response that references the actual data

::alert[This approach combines the best of both worlds: the conversational abilities of foundation models and the accuracy of querying actual business data.]{header="Note"}

### Step 4: Test Your Conversational Assistant ✅

1. Preview your application
2. Start a conversation with questions like:
   - "Which products are running low on inventory?"
   - "How are sales performing compared to last month?"
   - "What are our top-selling products in the West region?"
3. Observe how the assistant:
   - Maintains context throughout the conversation
   - Provides relevant business insights
   - Formats responses in a readable way

::alert[This conversational assistant demonstrates how AWS Bedrock and Superblocks can work together to create intuitive interfaces for business data that feel natural and helpful.]{header="Success" type="success"}

![Conversational Business Assistant](/images/business-assistant.png) -->
