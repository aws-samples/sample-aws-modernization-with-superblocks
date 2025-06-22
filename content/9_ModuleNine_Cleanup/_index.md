---
title: "Cleaning Up Resources"
chapter: true
weight: 10
---

After completing this workshop, it's important to clean up the resources you've created to avoid incurring any unexpected charges. This module will guide you through the process of properly removing all resources created during the workshop.

## What You'll Learn

In this module, you'll:

- Remove AWS Bedrock model access permissions
- Delete any AWS resources created during the workshop
- Clean up your Superblocks workspace (optional)

## Time to Complete

This module should take approximately 10 minutes to complete.

## AWS Resources Cleanup

### Step 1: Revoke Amazon Bedrock Model Access

1. Navigate to the Amazon Bedrock console: [https://console.aws.amazon.com/bedrock/](https://console.aws.amazon.com/bedrock/)
2. In the left navigation pane, select "Model access"
3. Uncheck the models you enabled during the workshop (Amazon and Anthropic)
4. Click "Update model access" to revoke access to these models

### Step 2: Use AWS CloudShell for Resource Cleanup

AWS CloudShell provides a browser-based shell with AWS CLI pre-installed, making it the recommended way to clean up resources.

1. Open AWS CloudShell by clicking the CloudShell icon in the AWS Management Console navigation bar
2. Once CloudShell loads, run the following commands to remove any resources created during the workshop:

```bash
# List all Amazon Bedrock model access permissions
aws bedrock list-foundation-models

# Remove any IAM roles created for the workshop (if applicable)
# aws iam delete-role --role-name superblocks-workshop-role

# Check for any CloudWatch Logs created during the workshop
aws logs describe-log-groups --query 'logGroups[?contains(logGroupName, `superblocks`) == `true`].[logGroupName]' --output table

# Delete any CloudWatch Log groups found
# aws logs delete-log-group --log-group-name /aws/lambda/superblocks-function
```

{{% notice warning %}}
Make sure you understand what each command does before executing it. The commands above are examples and may need to be adjusted based on the specific resources created during your workshop session.
{{% /notice %}}

## Superblocks Cleanup (Optional)

If you don't plan to continue using Superblocks after this workshop, you can clean up your Superblocks workspace:

1. Log in to your Superblocks account
2. Navigate to your workshop project
3. Go to "Settings" > "General"
4. Scroll to the bottom and click "Delete Project"
5. Type the project name to confirm deletion

{{% notice note %}}
If you're interested in continuing to use Superblocks, you can keep your workshop project as a reference or starting point for future development.
{{% /notice %}}

## Verification

To verify that all resources have been properly cleaned up:

1. Check the Amazon Bedrock console to ensure model access has been revoked
2. Review your AWS resources to confirm that no workshop-related resources remain
3. Verify that no unexpected charges appear on your AWS billing dashboard

## Next Steps

Congratulations on completing the workshop and properly cleaning up your resources! We hope you found this workshop valuable and that you're excited about the possibilities of building with Superblocks and AWS Bedrock.

If you're interested in learning more:

- Explore the [AWS Bedrock documentation](https://docs.aws.amazon.com/bedrock/)
- Check out the [Superblocks documentation](https://docs.superblocks.com)
- Visit the [AWS Marketplace listing for Superblocks](https://aws.amazon.com/marketplace/pp/prodview-kllccta3zgs2q) to learn about subscription options

Thank you for participating in this workshop!
