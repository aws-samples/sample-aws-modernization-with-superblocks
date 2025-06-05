---
title: "VS Code Server Setup"
chapter: true
weight: 2
---

# Setting Up VS Code Server (Self-Paced)

This guide will help you access your VS Code server environment when running the workshop independently.

## Prerequisites

- AWS Account with administrative access
- Successfully deployed vscode-server CloudFormation stack
- Modern web browser (Chrome, Firefox, or Edge recommended)

## Accessing Your VS Code Environment

### Step 1: Locate CloudFormation Outputs

1. Open the [AWS Management Console](https://console.aws.amazon.com)
2. Navigate to **CloudFormation** service
3. Select your `vscode-server` stack
4. Click the **Outputs** tab
5. Locate two key pieces of information:
   - **VSCODEURL**: Your VS Code server endpoint
   - **Password**: Your authentication credentials

![CloudFormation Outputs Location](/images/cloudformation-outputs.png)

### Step 2: Configure Access

1. Copy the **Password** from CloudFormation Outputs
2. Click the **VSCODEURL** value to open VS Code in a new tab
3. When prompted, paste your password
4. Select **Login** to access your environment

![VS Code Login Screen](/images/vscode-server-login.png)

### Step 3: Verify Your Environment

After logging in, confirm:

- File explorer is visible on the left
- Editor area is accessible in the center
- Terminal can be opened (`` Ctrl + ` ``)
- Git integration is working
- AWS credentials are configured properly

![VS Code Interface](/images/vscode-server-interface.png)

## Workshop Environment Setup

The VS Code environment has been pre-configured with:

1. **AWS CLI**: Pre-configured with credentials for your workshop account
2. **Node.js and npm**: For running the Superblocks application
3. **Git**: For version control
4. **Required Extensions**: AWS Toolkit and other helpful extensions

## Security Considerations

- Your VS Code server is protected by CloudFront
- Access is limited to your IP address
- Session cookies are encrypted
- All traffic is SSL/TLS encrypted
- Environment is isolated in your AWS account

## Troubleshooting Guide

### Common Issues and Solutions

1. **Cannot Access URL**
   - Verify CloudFormation stack deployed successfully
   - Check if CloudFront distribution is deployed
   - Ensure your IP is allowed in security groups

2. **Authentication Failed**
   - Double-check password from CloudFormation outputs
   - Clear browser cache and cookies
   - Try incognito/private browsing mode

3. **Performance Issues**
   - Check your internet connection
   - Verify instance type matches requirements
   - Monitor EC2 resource utilization

4. **AWS Credential Issues**
   - Verify IAM roles are properly configured
   - Check AWS CLI configuration
   - Ensure temporary credentials haven't expired

## Cost Considerations
- EC2 instance running costs
- CloudFront distribution charges
- Data transfer fees
- Storage costs for EBS volumes

{{% notice warning %}}
Remember to delete all resources by following the [Clean Up Resources](/9_ModuleNine_Cleanup) section when you're done with the workshop to avoid unnecessary charges.
{{% /notice %}}

## Next Steps

Now that you have access to the VS Code environment, you're ready to proceed to the [Module 1: Overview](/1_ModuleOne_Overview) section of the workshop.
