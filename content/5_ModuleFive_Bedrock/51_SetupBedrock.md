---
title: "Setting up AWS Bedrock"
weight: 1
---

## Configuring AWS Bedrock in Superblocks 🔌

Superblocks makes it remarkably simple to integrate with AWS Bedrock. In this section, we'll configure the connection between your Superblocks application and AWS Bedrock.

### Step 1: Configure AWS Credentials

First, we need to set up the AWS credentials that will allow Superblocks to access Bedrock:

1. In your Superblocks workspace, navigate to **Settings > Integrations**
2. Click on **+ Add Integration** and select **AWS**
3. Enter a name for your integration (e.g., "AWS-Bedrock")
4. Enter your AWS credentials:
   - AWS Access Key ID
   - AWS Secret Access Key
   - AWS Region (choose a region where Bedrock is available, such as `us-east-1`)
5. Click **Test Connection** to verify your credentials work
6. Click **Save** to store your integration

:::alert{header="Tip" type="info"}
For production environments, we recommend using IAM roles with appropriate permissions rather than access keys. For this workshop, we're using access keys for simplicity.
:::

### Step 2: Create a Bedrock Resource 🧩

Now, let's create a Bedrock resource in Superblocks:

1. Navigate to **Resources** in the left sidebar
2. Click **+ Create Resource** and select **AWS Bedrock**
3. Enter a name for your resource (e.g., "InventoryAnalysis")
4. Select the AWS integration you created in the previous step
5. Choose the foundation model you want to use:
   - For text analysis: **Claude 3 Sonnet**
   - For image generation: **Stable Diffusion XL**
6. Click **Create Resource**

::alert[Different foundation models excel at different tasks. Claude models are excellent for text analysis and reasoning, while Stable Diffusion excels at image generation. Choose the appropriate model for your specific use case.]{header="Note"}

### Step 3: Test Your Bedrock Connection ✅

Let's verify that your Bedrock connection is working:

1. Navigate to **API Builder** in the left sidebar
2. Click **+ Create API** and select **HTTP API**
3. Name your API "BedrockTest"
4. Add a new step and select **AWS Bedrock**
5. Configure the step:
   - Select your Bedrock resource
   - Choose "Text" as the input type
   - Enter the following prompt: "Summarize the benefits of AI for inventory management in 3 bullet points"
6. Click **Test** to run the API
7. You should see a response from the model with 3 bullet points about inventory management

::alert[Congratulations! You've successfully connected Superblocks to AWS Bedrock. In the next section, we'll build a more sophisticated feature using this integration.]{header="Success" type="success"}

![Bedrock Test Response](/images/bedrock-test-response.png)
