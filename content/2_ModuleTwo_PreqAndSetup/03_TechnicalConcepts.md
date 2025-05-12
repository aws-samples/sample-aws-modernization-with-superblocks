---
title: "Technical Concepts"
chapter: true
weight: 3
---

# Technical Concepts

## Superblocks Architecture

### Deployment Options

Superblocks offers two deployment models to meet different organizational needs:

1. **Cloud (Default)**
   - Simple to get started and manage
   - Superblocks Cloud acts as a proxy to your integration
   - No customer data storage in Superblocks

2. **On-premise Agent**
   - Customer data remains within your VPC
   - Ideal for strict compliance requirements
   - Uses lightweight open-source agent

### On-Premise Agent Architecture

The following diagram shows how the On-Premise Agent integrates with your infrastructure:

<div style="position: relative; padding-bottom: 2%; max-width: 800px; margin: 0 auto;">
  <img src="/images/opa-diagram.png" alt="On-Premise Agent Architecture" style="width: 100%; height: auto;"/>
</div>

### Core Components

| Component | Description |
|-----------|----------------------|
| **Superblocks Cloud**<br>(Control Plane) | • Manages user access and permissions across your organization<br>• Provisions and maintains agent security keys<br>• Provides centralized logging of all platform activities<br>• Handles user authentication and SSO integration |
| **On-Premise Agent**<br>(Compute Plane) | • Runs your APIs and functions within your own infrastructure<br>• Manages connections to your databases and internal services<br>• Scales automatically based on workload demands<br>• Ensures reliable execution with automatic retries |
| **Data Plane**<br>(APIs & Databases) | • Connects to your existing data sources (RDBMS, NoSQL, Data Warehouses)<br>• Supports bring-your-own-data model with no data storage in Superblocks<br>• Extensible with custom Python/JavaScript libraries for new integrations |


### Request Flow

The following diagram shows the request flow when using the On-Premise Agent:

<div style="position: relative; padding-bottom: 2%; max-width: 800px; margin: 0 auto;">
  <img src="/images/opa-flow-diagram.png" alt="Request Flow" style="width: 100%; height: auto;"/>
</div>

#### Executing Requests via the On-Premise Agent 

1. The browser makes a separate secure request to the on-premise agent inside of your network to execute an API
2. The agent securely retrieves the API definition from the closest cache on the Global Superblocks Edge Network
3. The agent executes the API inside your private network querying your databases or internal APIs and sends the data directly to the browser

## Next Steps
Now that you understand the Superblocks architecture and security features, we'll look at setting up the On-Premise Agent for secure database access.
