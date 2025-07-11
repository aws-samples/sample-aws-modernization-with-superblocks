---
title: "Cleaning Up Resources"
chapter: true
weight: 10
---

# 🧹 Comprehensive Cleanup

After completing this workshop, it's important to clean up all resources you've created to avoid incurring any unexpected charges. This module provides a comprehensive cleanup process to ensure all workshop resources are properly removed.

## 🚀 Creating and Running the Cleanup Script

Navigate to the AWS Management Console and ensure you're in the correct region where your resources were deployed. Then, open CloudShell by clicking the icon located in the top navigation bar:

![CloudShell](/images/cloudshell-icon.png)

Copy and paste the following commands to create, make executable, and run the cleanup script:

```bash
cat << 'EOF' > cleanup-workshop.sh
#!/bin/bash
echo "🧹 Starting comprehensive workshop cleanup..."

# Set AWS region if not already set
export AWS_REGION=${AWS_REGION:-$(aws configure get region)}
echo "🌎 Using AWS Region: $AWS_REGION"

# Find and delete CloudFormation stacks related to the workshop
echo "🔍 Finding CloudFormation stacks..."
STACKS_TO_DELETE=$(aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE --query "StackSummaries[?contains(StackName, 'superblocks') || contains(StackName, 'vscode-server')].StackName" --output text)

if [ -n "$STACKS_TO_DELETE" ]; then
  echo "🗑️ Deleting CloudFormation stack(s): $STACKS_TO_DELETE"
  for stack in $STACKS_TO_DELETE; do
    # First, disable deletion protection on RDS instances in the stack
    RDS_INSTANCES=$(aws cloudformation describe-stack-resources --stack-name $stack --query "StackResources[?ResourceType=='AWS::RDS::DBInstance'].PhysicalResourceId" --output text)
    for rds in $RDS_INSTANCES; do
      echo "🔓 Disabling deletion protection for RDS instance $rds..."
      aws rds modify-db-instance --db-instance-identifier $rds --no-deletion-protection --apply-immediately
    done
    
    # Now delete the stack
    echo "🗑️ Deleting stack $stack..."
    aws cloudformation delete-stack --stack-name $stack
    echo "⏳ Waiting for stack $stack to be deleted..."
    aws cloudformation wait stack-delete-complete --stack-name $stack
    echo "✅ Stack $stack deleted successfully"
  done
else
  echo "ℹ️ No matching CloudFormation stacks found"
fi

# Revoke Amazon Bedrock model access
echo "🔍 Checking for Bedrock model access..."
BEDROCK_MODELS=$(aws bedrock list-foundation-models --query "modelSummaries[?contains(modelId, 'anthropic') || contains(modelId, 'amazon')].modelId" --output text)

if [ -n "$BEDROCK_MODELS" ]; then
  echo "🗑️ Revoking access to Bedrock models..."
  for model in $BEDROCK_MODELS; do
    echo "🗑️ Revoking access to model $model..."
    aws bedrock delete-foundation-model-agreement --model-id $model || true
  done
else
  echo "ℹ️ No Bedrock model access to revoke"
fi

# Find and delete Secrets Manager secrets
echo "🔍 Finding Secrets Manager secrets..."
SECRETS=$(aws secretsmanager list-secrets --query "SecretList[?contains(Name, 'superblocks') || contains(Name, 'rds-credentials') || contains(Name, 'MyUserAccessKey')].Name" --output text)

if [ -n "$SECRETS" ]; then
  echo "🗑️ Deleting Secrets Manager secrets..."
  for secret in $SECRETS; do
    echo "🗑️ Deleting secret $secret..."
    aws secretsmanager delete-secret --secret-id $secret --force-delete-without-recovery
  done
else
  echo "ℹ️ No matching Secrets Manager secrets found"
fi

# Find and delete IAM roles and policies
echo "🔍 Finding IAM roles related to the workshop..."
IAM_ROLES=$(aws iam list-roles --query "Roles[?(!contains(RoleName, 'AWSServiceRole')) && (contains(RoleName, 'Superblocks') || contains(RoleName, 'Bedrock') || contains(RoleName, 'superblocks'))].RoleName" --output text)

if [ -n "$IAM_ROLES" ]; then
  echo "🗑️ Deleting IAM roles..."
  for role in $IAM_ROLES; do
    # Detach policies first
    POLICIES=$(aws iam list-attached-role-policies --role-name $role --query "AttachedPolicies[].PolicyArn" --output text)
    for policy in $POLICIES; do
      echo "🗑️ Detaching policy $policy from role $role..."
      aws iam detach-role-policy --role-name $role --policy-arn $policy
    done

    # Delete role
    echo "🗑️ Deleting IAM role $role..."
    aws iam delete-role --role-name $role
  done
else
  echo "ℹ️ No matching deletable IAM roles found"
fi

# Find and delete IAM users
echo "🔍 Finding IAM users related to the workshop..."
IAM_USERS=$(aws iam list-users --query "Users[?contains(UserName, 'Superblocks')].UserName" --output text)

if [ -n "$IAM_USERS" ]; then
  echo "🗑️ Deleting IAM users..."
  for user in $IAM_USERS; do
    # Delete access keys
    ACCESS_KEYS=$(aws iam list-access-keys --user-name $user --query "AccessKeyMetadata[].AccessKeyId" --output text)
    for key in $ACCESS_KEYS; do
      echo "🗑️ Deleting access key $key for user $user..."
      aws iam delete-access-key --user-name $user --access-key-id $key
    done
    
    # Remove from groups
    GROUPS=$(aws iam list-groups-for-user --user-name $user --query "Groups[].GroupName" --output text)
    for group in $GROUPS; do
      echo "🗑️ Removing user $user from group $group..."
      aws iam remove-user-from-group --user-name $user --group-name $group
    done
    
    # Delete user
    echo "🗑️ Deleting IAM user $user..."
    aws iam delete-user --user-name $user
  done
else
  echo "ℹ️ No matching IAM users found"
fi

# Find and delete IAM groups
echo "🔍 Finding IAM groups related to the workshop..."
IAM_GROUPS=$(aws iam list-groups --query "Groups[?contains(GroupName, 'Superblocks')].GroupName" --output text)

if [ -n "$IAM_GROUPS" ]; then
  echo "🗑️ Deleting IAM groups..."
  for group in $IAM_GROUPS; do
    # Detach policies first
    POLICIES=$(aws iam list-attached-group-policies --group-name $group --query "AttachedPolicies[].PolicyArn" --output text)
    for policy in $POLICIES; do
      echo "🗑️ Detaching policy $policy from group $group..."
      aws iam detach-group-policy --group-name $group --policy-arn $policy
    done
    
    # Delete group
    echo "🗑️ Deleting IAM group $group..."
    aws iam delete-group --group-name $group
  done
else
  echo "ℹ️ No matching IAM groups found"
fi

# Find and delete IAM policies
echo "🔍 Finding IAM policies related to the workshop..."
IAM_POLICIES=$(aws iam list-policies --scope Local --query "Policies[?contains(PolicyName, 'Bedrock') || contains(PolicyName, 'Superblocks')].Arn" --output text)

if [ -n "$IAM_POLICIES" ]; then
  echo "🗑️ Deleting IAM policies..."
  for policy in $IAM_POLICIES; do
    echo "🗑️ Deleting IAM policy $policy..."
    aws iam delete-policy --policy-arn $policy
  done
else
  echo "ℹ️ No matching IAM policies found"
fi

# Find and delete SSM parameters
echo "🔍 Finding SSM parameters related to the workshop..."
SSM_PARAMS=$(aws ssm describe-parameters --query "Parameters[?contains(Name, 'rds')].Name" --output text)

if [ -n "$SSM_PARAMS" ]; then
  echo "🗑️ Deleting SSM parameters..."
  for param in $SSM_PARAMS; do
    echo "🗑️ Deleting SSM parameter $param..."
    aws ssm delete-parameter --name $param
  done
else
  echo "ℹ️ No matching SSM parameters found"
fi

# Find and delete CloudWatch Log groups
echo "🔍 Finding CloudWatch Log groups..."
LOG_GROUPS=$(aws logs describe-log-groups --query "logGroups[?contains(logGroupName, 'superblocks') || contains(logGroupName, 'aws/ssm') || contains(logGroupName, '/aws/lambda/')].logGroupName" --output text)

if [ -n "$LOG_GROUPS" ]; then
  echo "🗑️ Deleting CloudWatch Log groups..."
  for log_group in $LOG_GROUPS; do
    echo "🗑️ Deleting Log group $log_group..."
    aws logs delete-log-group --log-group-name $log_group
  done
else
  echo "ℹ️ No matching CloudWatch Log groups found"
fi

echo "✅ Cleanup completed successfully!"
echo "🔍 Note: Please verify in the AWS Console that all resources have been properly deleted."
echo "💡 Tip: If you're interested in continuing to use Superblocks, you can sign up for a free account at https://app.superblocks.com/signup"
EOF

# Make the script executable
chmod +x cleanup-workshop.sh

# Run the cleanup script
./cleanup-workshop.sh
```

## 📋 What the Cleanup Script Does

The script automatically:

1. **Finds and deletes CloudFormation stacks** related to the workshop, including the VS Code server stack and RDS database stack
2. **Disables deletion protection on RDS instances** before attempting to delete the stack
3. **Revokes Amazon Bedrock model access** that was enabled during the workshop
4. **Removes Secrets Manager secrets** created for database credentials and IAM user access keys
5. **Deletes IAM roles, users, groups, and policies** created for Superblocks and Bedrock access
6. **Removes SSM parameters** used to store database connection information
7. **Cleans up CloudWatch Log groups** created during the workshop

## ⚠️ Manual Verification

After running the script, it's always a good practice to verify in the AWS Console that all resources have been properly deleted:

1. Check the CloudFormation console for any remaining stacks
2. Verify in the RDS console that all database instances have been removed
3. Check the IAM console for any remaining roles, users, groups, or policies
4. Verify in the Secrets Manager console that all secrets have been deleted
5. Check the SSM Parameter Store for any remaining parameters
6. Look for any remaining CloudWatch Log groups

## 🧹 Superblocks Cleanup (Optional)

If you don't plan to continue using Superblocks after this workshop, you can:

1. Log in to your Superblocks account at [https://app.superblocks.com](https://app.superblocks.com)
2. Navigate to your workspace settings
3. Select the option to delete your workspace
4. Follow the prompts to confirm deletion

Alternatively, you can reach out to Superblocks Support via email (help@superblocks.com) and request that your account be deleted.

{{% notice note %}}
If you're interested in continuing to use Superblocks, you can keep your workshop project as a reference or starting point for future development. Superblocks offers a free tier that allows you to continue exploring the platform.
{{% /notice %}}

## 🎉 Workshop Completion

Congratulations on completing the AWS and Superblocks workshop! You've learned how to:

- Set up a complete development environment with Superblocks
- Build frontend and backend applications
- Integrate with Amazon Bedrock for AI capabilities
- Implement governance and security best practices
- Deploy applications using Superblocks

We hope you found this workshop valuable and that you'll apply these techniques in your own projects!
