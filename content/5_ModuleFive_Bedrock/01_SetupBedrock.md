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

Now that you have enabled access to Amazon Bedrock models, you need to configure your Superblocks application to connect with Amazon Bedrock using Organization-level secrets for secure credential management.

1. **Navigate to Organization Settings:**

    - Click on your **name (email)** in the left menu of Superblocks
    - Select **Organization settings** from the dropdown menu

2. **Access Secrets Management:**

    - Scroll down to the **Build and Deploy** section
    - Click on **Secrets Management**

3. **Configure AWS Secrets Manager:**

    - Click **AWS Secrets Manager**
    - You'll be prompted to create a new AWS Secrets Manager configuration

4. **Enter Configuration Details:**

    - **Store Name**: `bedrock_credentials`
    - **Region**: `us-east-1`
    - **Auth Type**: **Access Key**

5. **Get Your Bedrock Credentials:**

    From your terminal or VSCode server, run:

    ```bash
    # Display your Amazon Bedrock credentials
    ./get-bedrock-credentials.sh
    ```

    This will show output similar to:
    ```
    === Bedrock Credentials for Superblocks Integration ===
    AWS Access Key ID: AKIA...
    AWS Secret Access Key: abc123...
    AWS Region: us-east-1
    ```

6. **Enter Your Credentials:**

    - **Access Key ID**: Enter the value from `AWS Access Key ID`
    - **Secret Access Key**: Enter the value from `AWS Secret Access Key`

![Superblocks AWS Secrets Manager Configuration](/images/superblocks-secrets-manager-config.png)

7. **Test and Create:**

    - Click **Test Connection** to verify the credentials work
    - Once the connection test is successful, click **Create**

8. **Verify Your Configuration:**

    After creating the AWS Secrets Manager configuration:

    - You should see your new secrets configuration listed in the Secrets Management section
    - The secrets will be accessible in your APIs using `{{sb_secrets.bedrock_credentials.SECRET_NAME}}`
    - This allows your Superblocks applications to securely access Amazon Bedrock credentials

:::alert{header="Security Best Practice" type="info"}
Using Organization-level secrets in Superblocks ensures that:
- Credentials are centrally managed and secure
- Multiple applications can use the same credential configuration
- Access is controlled at the organization level
- Credentials can be rotated without updating individual applications
- Secrets are only accessible in Backend APIs, not Frontend components
:::


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
