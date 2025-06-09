---
title: "Setting up Workshop Resources"
chapter: false
weight: 1
---

{{% notice warning %}}
Your account must have the ability to have Administrative Access
{{% /notice %}}

# Deploying the CloudFormation Templates  

To set up the required resources for this workshop, you will need to deploy two CloudFormation templates:

1. Download the :button[VS Code Server Template]{href="/static/infrastructure/vscode-server.yaml" action=download} CloudFormation Template
2. Download the :button[RDS Database with Bedrock Template]{href="/static/infrastructure/superblocks-rds-bedrock-template.yaml" action=download} CloudFormation Template

---

## Prerequisites  

Before deploying the templates, ensure the following:  

- **AWS account:** Must have an AWS Account with administrative permissions
- **AWS Management Console**: Log in to the [AWS Management Console](https://aws.amazon.com/console/)

---

## Deploying the Templates  

### Step 1: Deploy the `vscode-server.yaml`  

1. Navigate to the **CloudFormation** service in the [AWS Management Console](https://console.aws.amazon.com/cloudformation/).  
2. Click **Create stack** and select **Upload a template file**.

   ![Create Stack Button](/images/cloudformation-create-stack.png)  

3. Click **Choose file**, select the `vscode-server.yaml`, and click **Next**.  

   ![Upload Template](/images/cloudformation-upload-template.png)  

4. Enter a **Stack Name** (e.g., `vscode-server`).  
5. Review and modify the parameters as needed. Leave default values unless instructed otherwise.  

6. Click **Next**, then review the stack details.  
7. Acknowledge the **IAM role creation** if prompted by checking the appropriate box.  

   ![Acknowledge IAM Roles](/images/cloudformation-acknowledge-iam.png)  

8. Click **Submit** to start the deployment.  

   ![Create Stack Process](/images/cloudformation-create-process.png)

## Step 2: Deploy the RDS Database Template  

1. Navigate to the **CloudFormation** service in the [AWS Management Console](https://aws.amazon.com/console/)
2. Click **Create stack** and select **Upload a template file**

   ![Create Stack Button](/images/cloudformation-create-stack.png)  

3. Click **Choose file**, select the `superblocks-rds-bedrock-template.yaml` file you downloaded, and click **Next**
4. Enter a **Stack Name** (e.g., `superblocks-rds-stack`) and click **Next**
5. On the **Configure stack options** page, click **Next**
6. Review the stack details and acknowledge the IAM role creation by checking the appropriate box

   ![Acknowledge IAM Roles](/images/cloudformation-acknowledge-iam.png)  

7. Click **Create stack** to start the deployment

8. Wait for the stack status to change to `CREATE_COMPLETE` (this may take 5-15 minutes)


---

## Step 3: Verify the Deployments

1. Confirm that both the `superblocks-rds-stack` and the `vscode-server` stack has deployed successfully.

2. Go to the **CloudFormation Outputs** tab for the `VS Code Server` stack to get the access details

   ![CloudFormation Outputs Tab](/images/cloudformation-outputs.png)  

3. The `vscode-server.yaml` stack will provide the `VSCODEURL` and `Password` for accessing the VS Code server

4. Click on the `VSCODEURL` link to open the VS Code Server in your browser

5. Enter the password from the CloudFormation outputs when prompted

   ![VS Code Login Screen](/images/vscode-server-login.png)

6. You should now have access to the VS Code Server environment

   ![VS Code Interface](/images/vscode-server-interface.png)

---

## Step 4: Configure Environment Variables

The VS Code Server environment needs to be configured with the database connection details and Bedrock credentials:

1. Open a terminal in VS Code Server (press `` Ctrl + ` ``)

2. Run the following commands to set up environment variables:

```bash
# Get database endpoint from SSM Parameter Store
export DB_ENDPOINT=$(aws ssm get-parameter --name /rds/db-endpoint --query 'Parameter.Value' --output text)

# Get database username from SSM Parameter Store
export DB_USERNAME=$(aws ssm get-parameter --name /rds/db-username --query 'Parameter.Value' --output text)

# Get database password from SSM Parameter Store (with decryption)
export DB_PASSWORD=$(aws ssm get-parameter --name /rds/db-password --query 'Parameter.Value' --output text --with-decryption)

# Set database name
export DB_NAME="acme"

# Set PostgreSQL password environment variable
export PGPASSWORD=$DB_PASSWORD

# Get full connection string from SSM Parameter Store
export PROD_DATABASE_URL=$(aws ssm get-parameter --name /rds/connection-string --query 'Parameter.Value' --output text --with-decryption)

# Get Bedrock credentials from SSM Parameter Store
export BEDROCK_ACCESS_KEY_ID=$(aws ssm get-parameter --name /bedrock/access-key-id --query 'Parameter.Value' --output text)
export BEDROCK_SECRET_ACCESS_KEY=$(aws ssm get-parameter --name /bedrock/secret-access-key --query 'Parameter.Value' --output text --with-decryption)

# Add these to .bashrc for persistence
echo "export DB_ENDPOINT='$DB_ENDPOINT'" >> ~/.bashrc
echo "export DB_USERNAME='$DB_USERNAME'" >> ~/.bashrc
echo "export DB_PASSWORD='$DB_PASSWORD'" >> ~/.bashrc
echo "export DB_NAME='acme'" >> ~/.bashrc
echo "export PGPASSWORD='$DB_PASSWORD'" >> ~/.bashrc
echo "export PROD_DATABASE_URL='$PROD_DATABASE_URL'" >> ~/.bashrc
echo "export BEDROCK_ACCESS_KEY_ID='$BEDROCK_ACCESS_KEY_ID'" >> ~/.bashrc
echo "export BEDROCK_SECRET_ACCESS_KEY='$BEDROCK_SECRET_ACCESS_KEY'" >> ~/.bashrc

# Verify the environment variables
env | grep -E "DB_|PROD_DATABASE_URL|BEDROCK_"
```

---

**Note**: If you encounter errors during the deployment, ensure that:  

1. You have sufficient permissions to deploy CloudFormation stacks
2. Your AWS account has the required service limits for the resources being deployed
3. All parameters are correctly configured as per the workshop requirements

If you have any issues, refer to the troubleshooting section or contact the support team.
