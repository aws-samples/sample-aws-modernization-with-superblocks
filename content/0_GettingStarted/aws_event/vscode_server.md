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

![Event Outputs Location](/images/vscode-server-creds.png)

### Step 2: Launch VS Code Server

1. Click the **VSCODEURL** link in Event Outputs
2. A new browser tab will open with the VS Code login screen
3. Copy the password from Event Outputs
4. Paste it into the password field
5. Click **Login**

![VS Code Login Screen](/images/vscode-server-login.png)

### Step 3: Verify Your Connection

Once logged in, you should see the VS Code interface:
- A file explorer on the left
- An editor panel in the center
- A terminal at the bottom (if not visible, press `` Ctrl + ` `` to open it)

![VS Code Interface](/images/vscode-server-interface.png)

## Workshop Environment Setup

The VS Code environment has been pre-configured with:

1. **AWS CLI**: Pre-configured with credentials for your workshop account
2. **PostgreSQL Client**: For connecting to the workshop database
3. **Node.js and npm**: For running the Superblocks application
4. **Git**: For version control
5. **Required Extensions**: AWS Toolkit and other helpful extensions

## Database Connection

The environment is already configured with environment variables for connecting to the PostgreSQL database:

- **DB_ENDPOINT**: The database endpoint
- **DB_USERNAME**: The database username
- **DB_PASSWORD**: The database password
- **DB_NAME**: The database name (acme)
- **PROD_DATABASE_URL**: The full connection string

You can verify these environment variables by running the following command in the terminal:

```bash
env | grep -E "DB_|PROD_DATABASE_URL"
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
