---
title: "On-Premise Agent (Optional)"
chapter: true
weight: 4
---

# Local On-Premise Agent (OPA) Setup

## Overview

The On-Premise Agent (OPA) allows you to securely execute queries and access local services during development. When running locally, the OPA:

- Runs as a Docker container on your machine
- Provides secure access to local databases and services
- Maintains data privacy by processing requests within your environment

![Local OPA Deployment](/images/local-opa-deployment.png?width=45pc)

{{% notice info %}}
The On-Premise Agent is optional for this workshop and can be skipped.
{{% /notice %}}

## Prerequisites

Before starting, ensure you have:

- Docker Desktop installed and running
- Terminal/Command Prompt access
- A Superblocks account with admin access

## Setup Process

### Step 1: Generate an Agent Token

First, create a secure token for your local agent:

1. In Superblocks, navigate to **Organization Settings** → **Access Tokens**
2. Click **Create token**
3. Configure the token:

    - Name: "Local Development Agent"
    - Type: **Agent key**
    - Expiration: 90 days (default)

4. **Important**: Save the generated token securely - it cannot be viewed again

### Step 2: Launch the Agent

Run this command in your terminal, replacing `{AGENT_KEY}` with your token:

```bash
# Download and start the OPA container
curl -s https://raw.githubusercontent.com/superblocksteam/agent/main/compose.yaml | \
SUPERBLOCKS_AGENT_KEY="{AGENT_KEY}"  \
SUPERBLOCKS_AGENT_HOST_URL="http://localhost:8080" \
SUPERBLOCKS_AGENT_TAGS="profile:*" \
SUPERBLOCKS_DOCKER_AGENT_TAG="latest" \
SUPERBLOCKS_AGENT_DATA_DOMAIN="app.superblocks.com" \
docker compose -p superblocks -f - up
```

### Step 3: Connect Local Services

To connect your local databases or services:

1. Open the **Integrations** page in Superblocks
2. Select your integration type
3. Click **Manage** (⋮ menu)
4. Configure the connection:

    - Host: Use `host.docker.internal` instead of `localhost`
    - Port: Use the service's exposed port
    - Credentials: Enter your local service credentials


## Verification

### Step 4: Check Agent Status

1. Go to **Organization Settings** → **On-Premise Agents**
2. Switch to **On-Premise Deployment** view
3. Look for your agent with **Active** status

### Step 5: Test the Connection

1. Create a new API in Superblocks
2. Select your local integration
3. Run a test query
4. Verify the results in the response panel

## Troubleshooting

If you encounter issues:

1. Check the [Superblocks documentation](https://docs.superblocks.com)
2. Review agent logs for specific errors
3. Check out the [Troubleshooting OPA](https://docs.superblocks.com/superblocks/on-premise-agent/troubleshooting) guide for common issues and solutions.

{{% notice warning %}}
The On-Premise Agent is optional for this workshop. If you're using the cloud offering with a cloud database, you can skip this section.
{{% /notice %}}
