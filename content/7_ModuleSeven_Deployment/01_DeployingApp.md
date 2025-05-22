---
title: "Deploying Your Application"
chapter: true
weight: 1
---

# Deploying Your Application

Let's deploy your dashboard to production using Superblocks' simple deployment process.

## Understanding Deployment

When you deploy with Superblocks:
- Your application runs on our Global Edge Network
- Changes apply instantly worldwide
- Users get fast access from anywhere

## Deploying Your Dashboard

1. On the top right, click the down arrow next to "Preview"
2. Enter in a description for your changes
3. Click "Deploy" to publish your application
4. Click "View deployed app" to open your production dashboard

## Additional Deployment Options

While UI deployment is recommended for this workshop, Superblocks also supports automated deployment through CI/CD:

### GitHub Actions Integration

For production environments, you can automate deployment using actions:

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

### Command Line Deployment

Superblocks also provides a CLI to deploy your application. This method works whether your tools are connected to git or not. 

## Next Steps
Congratulations! Your Acme Incorporated dashboard is now live and deployed! Let's look at what else you can do next.
