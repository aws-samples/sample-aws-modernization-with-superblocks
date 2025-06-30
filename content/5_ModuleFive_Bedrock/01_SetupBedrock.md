---
title: "Setting up Amazon Bedrock"
chapter: true
weight: 1
---

Let's configure the connection between your Superblocks application and Amazon Bedrock. In this section, you'll learn how to enable access to Amazon Bedrock and request access to specific foundation models that we'll use in our application.

## Step 1: Navigate to the Amazon Bedrock Console

1. Sign in to the AWS Management Console
2. Navigate directly to the Amazon Bedrock console using this link: [https://console.aws.amazon.com/bedrock/](https://console.aws.amazon.com/bedrock/)
3. If this is your first time accessing Amazon Bedrock, you'll see a welcome page. Click "Get started" to proceed.

![Bedrock Welcome Page](/images/bedrock-welcome-page.png)

## Step 2: Request Model Access

Before you can use Amazon Bedrock models, you need to request access to them. Follow these steps:

1. In the Amazon Bedrock console, navigate to the left sidebar and select "Model access" or go directly to the Model access page using this link: [https://console.aws.amazon.com/bedrock/home#/modelaccess](https://console.aws.amazon.com/bedrock/home#/modelaccess)

2. You'll see a page with the heading "What is Model access?" explaining that to use Bedrock serverless models, account users with the correct IAM permissions must enable access to available Bedrock foundation models (FMs).

3. You'll be presented with two options:

    - **Enable all models** - Grants access to all available foundation models
    - **Enable specific models** - Allows you to select which models to enable

4. For this workshop, select **Enable specific models** so we can choose exactly what we need.

5. After selecting "Enable specific models", you'll see a list of available model providers. Click the **Collapse all** button at the top of the list to make selection easier.

6. For this workshop, we only need to select two model providers:

    - **Amazon** - Select the checkbox next to "Amazon" to get access to the **Amazon Nova** family of models, AWS's own state-of-the-art foundation models (including Amazon Nova Premier, Amazon Nova Pro, Amazon Nova Lite, and Amazon Nova Micro)
    - **Anthropic** - Select the checkbox next to "Anthropic"

   You can experiment with other models if you wish, but these two are sufficient for completing the workshop exercises.

![Select Models](/images/bedrock-select-models.png)

7. If you want to be more selective about specific models, you can expand each provider:

    - Under **Anthropic**, select **Claude 3.5 Sonnet** - This is Anthropic's powerful model for complex reasoning and content generation
    - Under **Amazon**, select **Amazon Nova Premier** - This is Amazon's most capable multimodal model for complex tasks

   However, selecting the entire provider (as in step 6) will give you access to all models from that provider, which gives you more flexibility during the workshop.

8. At the bottom of the page, you'll see a note about Amazon Bedrock Quotas with a link to view the default quotas and limits that apply to Amazon Bedrock. This is useful information, but you don't need to change any quotas for this workshop.

9. Click the "Next" button at the bottom of the page

10. In the confirmation dialog, review your selections and click the "Submit" button

11. You'll see a notification that your request is being processed. This typically takes only a few minutes.

12. Wait for the status of each model family to change from "Access requested" to "Access granted"

![Access Granted](/images/bedrock-access-granted.png)

## Step 3: Verify Model Access

Once access has been granted, let's verify that you can see the individual models:

1. In the left sidebar, select "Model playground" or go directly to the Text generation playground using this link: [https://console.aws.amazon.com/bedrock/home#/text-generation-playground](https://console.aws.amazon.com/bedrock/home#/text-generation-playground)

2. In the dropdown menu at the top, you should now be able to select from the models you've enabled
3. Try selecting different models to verify access:

    - **Anthropic Claude 3.5 Sonnet** - A powerful model for complex reasoning and content generation
    - **Amazon Nova Premier** - Amazon's most capable multimodal model for complex tasks

4. You can also use the compare mode to test how different models respond to the same prompt:

    - Click the "Select model" button at the top of the playground
    - Choose one of the models that have been granted access
    - Enter a prompt in the text area
    - Click "Run" to see the response
    - Add another model for comparison by clicking "Add model" and selecting a different model
    - This allows you to directly compare how different models handle the same prompt

![Model Playground](/images/bedrock-model-playground.png)

## Step 4: Configure Superblocks to Connect with Amazon Bedrock

Now that you have enabled access to Amazon Bedrock models, you need to configure your Superblocks application to connect with Amazon Bedrock using the AWS Identity and Access Management (IAM) credentials we set up earlier:

1. First, let's retrieve your AWS Bedrock credentials from the environment variables:

```bash
# Display your Bedrock credentials for easy copying
echo "AWS Access Key ID: $BEDROCK_ACCESS_KEY_ID"
echo "AWS Secret Access Key: $BEDROCK_SECRET_ACCESS_KEY"
echo "AWS Region: us-east-1"  # Default region for this workshop
```

2. Copy these credentials as you'll need them in the next steps.

3. In the Superblocks application:

    - Navigate to the [Integrations](https://app.superblocks.com/integrations) section from the left sidebar
    - Click the **+ Add Integration** button
    - Search for and select **AWS**

4. In the AWS integration form:

    - **Name**: Enter "Amazon Bedrock"
    - **AWS Access Key ID**: Paste the value of `BEDROCK_ACCESS_KEY_ID` you copied earlier
    - **AWS Secret Access Key**: Paste the value of `BEDROCK_SECRET_ACCESS_KEY` you copied earlier
    - **AWS Region**: Enter "us-east-1" (or the region where you enabled Bedrock models)
    - Click **Test Connection** to verify the credentials work
    - Click **Save** to create the integration

![Superblocks AWS Integration](/images/superblocks-aws-integration.png)

5. Verify the integration is working:

    - You should see a green checkmark next to your new AWS integration
    - This integration will now be available to use in your Superblocks applications


## Step 5: Understanding Model Pricing

Before proceeding, it's important to understand the pricing model for Amazon Bedrock:

- Amazon Bedrock uses a pay-as-you-go pricing model
- You're charged based on the number of input and output tokens processed
- Different models have different pricing tiers
- **Amazon Nova models** offer competitive pricing with excellent performance, making them a cost-effective choice for many applications
- For this workshop, the usage will be minimal and should fall within the AWS Free Tier limits for new accounts

For the most current pricing information, refer to the [Amazon Bedrock pricing page](https://aws.amazon.com/bedrock/pricing/).

## Next Steps

Now that you have successfully enabled access to Amazon Bedrock models, you're ready to integrate them with your Superblocks application. In the next section, we'll configure the connection between Superblocks and Amazon Bedrock.
