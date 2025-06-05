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
2. Download the :button[RDS Database Template]{href="/static/infrastructure/superblocks-rds-template.yaml" action=download} CloudFormation Template

---

## Prerequisites  

Before deploying the templates, ensure the following:  

- **AWS account:** Must have an AWS Account with administrative permissions
- **AWS Management Console**: Log in to the [AWS Management Console](https://aws.amazon.com/console/)

---

## Step 1: Deploy the RDS Database Template  

1. Navigate to the **CloudFormation** service in the [AWS Management Console](https://aws.amazon.com/console/)
2. Click **Create stack** and select **Upload a template file**

   ![Create Stack Button](/images/cloudformation-create-stack.png)  

3. Click **Choose file**, select the `superblocks-rds-template.yaml` file you downloaded, and click **Next**

   ![Upload Template](/images/cloudformation-upload-template.png)  

4. Enter a **Stack Name** (e.g., `superblocks-rds-stack`) and click **Next**
5. On the **Configure stack options** page, click **Next**
6. Review the stack details and acknowledge the IAM role creation by checking the appropriate box

   ![Acknowledge IAM Roles](/images/cloudformation-acknowledge-iam.png)  

7. Click **Create stack** to start the deployment

8. Wait for the stack status to change to `CREATE_COMPLETE` (this may take 5-10 minutes)

## Step 2: Deploy the VS Code Server Template  

1. Navigate back to the **CloudFormation** service
2. Click **Create stack** and select **Upload a template file**
3. Click **Choose file**, select the `vscode-server.yaml` file you downloaded, and click **Next**
4. Enter a **Stack Name** (e.g., `vscode-server-stack`) and do not modify any **Parameters**
5. On the **Configure stack options** page, click **Next**
6. Review the stack details and acknowledge the IAM role creation by checking the appropriate box
7. Click **Create stack** to start the deployment
8. Wait for the stack status to change to `CREATE_COMPLETE` (this may take 10-15 minutes)

---

## Step 3: Verify the Deployment

1. Go to the **CloudFormation Outputs** tab for the VS Code Server stack to get the access details

   ![CloudFormation Outputs Tab](/images/cloudformation-outputs.png)  

2. The `vscode-server.yaml` stack will provide the `VSCODEURL` and `Password` for accessing the VS Code server

3. Click on the `VSCODEURL` link to open the VS Code Server in your browser

4. Enter the password from the CloudFormation outputs when prompted

   ![VS Code Login Screen](/images/vscode-server-login.png)

5. You should now have access to the VS Code Server environment

   ![VS Code Interface](/images/vscode-server-interface.png)

---

## Step 4: Configure Environment Variables

The VS Code Server environment needs to be configured with the database connection details:

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

# Add these to .bashrc for persistence
echo "export DB_ENDPOINT='$DB_ENDPOINT'" >> ~/.bashrc
echo "export DB_USERNAME='$DB_USERNAME'" >> ~/.bashrc
echo "export DB_PASSWORD='$DB_PASSWORD'" >> ~/.bashrc
echo "export DB_NAME='acme'" >> ~/.bashrc
echo "export PGPASSWORD='$DB_PASSWORD'" >> ~/.bashrc
echo "export PROD_DATABASE_URL='$PROD_DATABASE_URL'" >> ~/.bashrc

# Verify the environment variables
env | grep -E "DB_|PROD_DATABASE_URL"
```

---

**Note**: If you encounter errors during the deployment, ensure that:  

1. You have sufficient permissions to deploy CloudFormation stacks
2. Your AWS account has the required service limits for the resources being deployed
3. All parameters are correctly configured as per the workshop requirements

If you have any issues, refer to the troubleshooting section or contact the support team.
