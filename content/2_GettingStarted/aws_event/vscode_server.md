---
title: "VS Code Server Setup"
chapter: true
weight: 2
---

# Setting Up VS Code Server Access

In this section, you'll learn how to access the cloud-based VS Code environment that we'll use throughout the workshop.

## Prerequisites

- A modern web browser (Chrome, Firefox, or Edge recommended)
- Workshop credentials provided by your instructor

## Accessing the VS Code Server

### Step 1: Locate Your Credentials

1. Navigate to the **Event Outputs** section in the workshop portal
2. Look for two important pieces of information:

    - **VSCODEURL**: Your unique VS Code server URL
    - **Password**: Your secure access credentials

![Event Outputs Location](/images/Vscode-server-creds.png)

### Step 2: Launch VS Code Server

1. Click the **VSCODEURL** link in Event Outputs
2. A new browser tab will open with the VS Code login screen
3. Copy the password from Event Outputs
4. Paste it into the password field
5. Click **Login**

![VS Code Login Screen](/images/Vscode-server-login.png)

### Step 3: Verify Your Connection

Once logged in, you should see the VS Code interface:
- A file explorer on the left
- An editor panel in the center
- A terminal at the bottom (if not visible, press `` Ctrl + ` `` to open it)

![VS Code Interface](/images/Vscode-server-interface.png)

## Workshop Environment Setup

The VS Code environment has been pre-configured with:

1. **AWS CLI**: Pre-configured with credentials for your workshop account
2. **PostgreSQL Client**: For connecting to the workshop database
3. **Node.js and npm**: For running the Superblocks application
4. **Git**: For version control
5. **Required Extensions**: AWS Toolkit and other helpful extensions

## Configure Environment Variables

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

# Get Bedrock credentials from AWS Secrets Manager
export BEDROCK_ACCESS_KEY_ID=$(aws secretsmanager get-secret-value --secret-id MyUserAccessKey --query 'SecretString' --output text | jq -r '.AccessKeyId')
export BEDROCK_SECRET_ACCESS_KEY=$(aws secretsmanager get-secret-value --secret-id MyUserAccessKey --query 'SecretString' --output text | jq -r '.SecretAccessKey')

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

## Important Notes

- The VS Code server is secured through CloudFront for enhanced security
- Your session will remain active throughout the workshop
- All necessary extensions are pre-installed
- Work is automatically saved but will be lost when the workshop ends

## Troubleshooting

If you experience issues:
1. Clear your browser cache
2. Try using an incognito/private window
3. Ensure your corporate firewall isn't blocking the connection
4. Contact the workshop support team if problems persist

## Next Steps

Now that you have access to the VS Code environment, you're ready to proceed to the next step to set up the database.
