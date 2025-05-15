---
title: "On-Premise Agent (Optional)"
chapter: true
weight: 4
---

# On-Premise Agent Setup

The Superblocks On-Premise Agent (OPA) allows you to securely connect to resources in your local environment. This section will guide you through setting up OPA locally.

![Local OPA Deployment](/images/local-opa-deployment.png?width=45pc)

## Prerequisites

Before installing the OPA, ensure you have:

1. Docker installed on your machine
2. Access to a terminal/command prompt
3. An agent token from Superblocks

## Installation Steps

### 1. Get Your Agent Token

1. In Superblocks, go to **Organization Settings** → **Access Tokens**
2. Click **Create token**
3. Name your token (e.g., "Local Agent Token")
4. Keep the **expiration date** as the default (90 days)
5. For token type, select **Agent key**
6. Copy the generated token - **store it securely as it won't be shown again**

### 2. Deploy with Docker

Run the following command, replacing {AGENT_KEY} with the token you just generated.

```bash
curl -s https://raw.githubusercontent.com/superblocksteam/agent/main/compose.yaml | \
SUPERBLOCKS_AGENT_KEY="{AGENT_KEY}"  \
SUPERBLOCKS_AGENT_HOST_URL="http://localhost:8080" \
SUPERBLOCKS_AGENT_TAGS="profile:*" \
SUPERBLOCKS_DOCKER_AGENT_TAG="latest" \
SUPERBLOCKS_AGENT_DATA_DOMAIN="app.superblocks.com" \
docker compose -p superblocks -f - up
```

You should see Docker downloading images and starting containers. Keep this terminal window open to view logs.

## Configuration with local services

With the agent running, you can connect to locally hosted services as well:

1. Go to the Integrations page and select which integrations to connect locally
2. Click on ... followed by **Manage**
3. Fill out the configuration form for your local server/database
4. **Important:** For host addresses, use host.docker.internal instead of localhost to reach services from inside the Docker container

## Validating the Connection

1. In Superblocks, go to **Organization Settings** → **On-Premise Agents**
2. Change the view from **Cloud Deployment** to **On-Premise Deployment** using the dropdown
3. You should see your OPA with an **Active** status

### Getting Help

If you encounter issues:

1. Check the [Superblocks documentation](https://docs.superblocks.com)
2. Review agent logs for specific errors
3. Check out the [Troubleshooting OPA](https://docs.superblocks.com/superblocks/on-premise-agent/troubleshooting) guide for common issues and solutions.

{{% notice warning %}}
The On-Premise Agent is optional for this workshop. If you're using the cloud offering with a cloud database, you can skip this section.
{{% /notice %}}
