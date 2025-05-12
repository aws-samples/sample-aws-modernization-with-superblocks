---
title: "Deploying Your Application"
chapter: true
weight: 1
---

# Deploying Your Application

In this section, you'll learn how to deploy your Superblocks application and make changes available to all users. Superblocks provides multiple deployment methods that seamlessly integrate with your SDLC workflow.

## Understanding Deployment

When deploying in Superblocks:
- You'll select a specific commit to deploy
- The chosen commit becomes the version served to end-users
- Code is served through Superblocks Global Edge Network for optimized performance

{{% notice info %}}
When using Source Control, only commits on the default branch can be deployed. Make sure to merge your changes to make them deployable.
{{% /notice %}}

## Deployment Methods

### 1. Using Superblocks UI

1. Navigate to Version Control:
   - Click on the Version Control panel icon
   - Scroll to the "Committed changes" section

2. Deploy Your Changes:
   - Click the menu icon for your commit
   - Select "Deploy"
   - Review and confirm the deployment

### 2. Continuous Deployment (GitHub Actions)

1. Set Up GitHub Workflow:
   - Navigate to your repo's Actions tab
   - Create a new workflow
   - Use the following configuration:

```yaml
name: Deploy

on:
  workflow_run:  
    workflows: ["Sync changes to Superblocks"] 
    types:  
      - completed

jobs:
  deploy:
    if: ${{ github.event.workflow_run.head_branch == 'main' && github.event.workflow_run.conclusion == 'success' }} 
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Deploy
        uses: superblocksteam/deploy-action@v1
        id: deploy
        with:
          token: ${{ secrets.SUPERBLOCKS_TOKEN }}
```

### 3. Using Superblocks CLI

1. Install and Set Up:
```bash
# Install the CLI globally
npm i -g @superblocksteam/cli

# Log in to the CLI
superblocks login
```

2. Export Your Tools:
```bash
# Create and navigate to tools directory
mkdir superblocks_tools
cd superblocks_tools

# Export your tools
superblocks init [RESOURCE_URL]
```

3. Deploy Changes:
```bash
# Deploy the most recent commit
superblocks deploy

# Or deploy a specific commit
superblocks deploy --commit-id <COMMIT_ID>
```

## Best Practices

1. Always review changes before deployment
2. Use continuous deployment for automated workflows
3. Maintain proper version control hygiene

{{% notice tip %}}
The Superblocks Global Edge Network ensures optimal performance for users worldwide, regardless of their location.
{{% /notice %}}

## Next Steps
In the next section, we'll explore monitoring your deployed application and managing different environments.
