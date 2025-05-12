---
title: "On-Premise Agent (Optional)"
chapter: true
weight: 4
---

# On-Premise Agent Setup

The Superblocks On-Premise Agent (OPA) allows you to securely connect to resources in your local environment. This section will guide you through setting up OPA locally.

![Local OPA Deployment](/images/local-opa-deployment.png?width=45pc)


## Prerequisites

Before installing OPA, ensure you have:

1. Docker installed on your machine
2. Access to a terminal/command prompt
3. An agent token from Superblocks

## Installation Steps

### 1. Get Your Agent Token

1. In Superblocks, go to **Settings** → **On-Premise Agent**
2. Click **Create New Agent**
3. Name your agent (e.g., "Local Workshop Agent")
4. Copy the generated token

### 2. Deploy with Docker

```bash
docker run -d \
  --name superblocks-agent \
  -e AGENT_TOKEN=<your-agent-token> \
  -e WORKSPACE_ID=<your-workspace-id> \
  superblocks/agent:latest
```

### 3. Verify Installation

1. Check container status:
```bash
docker ps | grep superblocks-agent
```

2. View agent logs:
```bash
docker logs superblocks-agent
```

## Configuration

### Environment Variables

- `AGENT_TOKEN`: Your agent authentication token
- `WORKSPACE_ID`: Your Superblocks workspace ID
- `LOG_LEVEL` (optional): Set to `debug` for verbose logging

### Network Configuration

The agent needs outbound access to:
- `api.superblocks.com` on port 443
- Your local resources (database, services, etc.)

## Testing the Connection

1. In Superblocks, go to **Integrations**
2. Click **Add New Integration**
3. Select **PostgreSQL**
4. Choose your local agent in the connection settings
5. Test the connection

### Getting Help

If you encounter issues:
1. Check the [Superblocks documentation](https://docs.superblocks.com)
2. Review agent logs for specific errors
3. Check out the [Troubleshooting OPA](https://docs.superblocks.com/superblocks/on-premise-agent/troubleshooting) guide for common issues and solutions.

{{% notice warning %}}
The On-Premise Agent is optional for this workshop. If you're using the cloud offering with a cloud database, you can skip this section.
{{% /notice %}}
