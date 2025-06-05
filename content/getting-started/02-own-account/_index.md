---
title: "...on your own"
chapter: true
weight: 2
---

# Running the workshop on your own

{{% notice warning %}}
Only complete this section if you are running the workshop on your own. If you are at an AWS hosted event (such as re:Invent, Kubecon, Immersion Day, etc.), go to [Start the workshop at an AWS event](../01-aws-event/).
{{% /notice %}}

## Prerequisites

To run this workshop on your own, you'll need:

1. An AWS account with administrative permissions
2. A modern web browser (Chrome, Firefox, Safari, or Edge)
3. Basic familiarity with AWS services

## Setting up your environment

### Step 1: Sign in to your AWS account

1. Go to the [AWS Management Console](https://console.aws.amazon.com/)
2. Sign in with your credentials

### Step 2: Launch AWS CloudShell

1. In the AWS Management Console, click on the CloudShell icon in the navigation bar at the top of the screen
2. Wait for CloudShell to initialize (this may take a few moments)

### Step 3: Clone the workshop repository

Once CloudShell is ready, run the following commands:

```bash
git clone https://github.com/aws-samples/superblocks-workshop.git
cd superblocks-workshop
```

### Step 4: Set up your environment

Run the setup script to configure your environment:

```bash
./setup.sh
```

This script will:
- Create necessary IAM roles and policies
- Configure AWS Bedrock model access
- Set up sample data for the workshop

### Step 5: Access the workshop materials

The workshop materials are now available in your CloudShell environment. You can proceed with the workshop by following the instructions in each module.

{{% notice info %}}
Remember to clean up all resources when you're done with the workshop to avoid unnecessary charges. Follow the [Clean Up Resources](/9_ModuleNine_Cleanup) section for detailed instructions.
{{% /notice %}}
