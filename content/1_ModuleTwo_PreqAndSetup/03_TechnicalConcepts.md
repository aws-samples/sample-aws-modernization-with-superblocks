---
title: "Technical Concepts"
chapter: true
weight: 3
---

## Superblocks Architecture

Superblocks uses a modern three-plane architecture:

- **Control Plane** (Superblocks Cloud)
  - Handles user authentication and permissions
  - Manages application definitions and configurations
  - Provides centralized logging and monitoring

- **Compute Plane** (Execution Layer)
  - Executes API calls and functions
  - Processes data transformations
  - Handles request routing and retries

- **Data Plane** (Your Data Sources)
  - Your databases (SQL, NoSQL)
  - Internal services and APIs
  - Data warehouses and storage

## Deployment Options

You can deploy Superblocks in two ways, choosing where to run the compute plane based on your security needs:

1. **Cloud (Default)**

    - Compute plane runs in Superblocks' secure infrastructure
    - Zero setup with instant deployment
    - Data flows through but is never stored

2. **On-premise Agent**

    - Compute plane runs inside your VPC via lightweight agent
    - All data processing stays in your network
    - Open-source agent for full auditability

### On-Premise Agent Architecture

The following diagram shows how the On-Premise Agent integrates with your infrastructure:


![On-Premise Agent Architecture](/images/opa-diagram.png)

### Request Flow

The following diagram shows the request flow when using the On-Premise Agent:

![Request Flow](/images/opa-flow-diagram.png)

### Executing Requests via the On-Premise Agent

1. The browser makes a separate secure request to the on-premise agent inside of your network to execute an API or database call
2. The agent securely retrieves the API definition from the closest cache on the Global Superblocks Edge Network
3. The agent executes the API inside your private network querying your databases or internal APIs and sends the data directly to the browser

## Next Steps

Now that you understand the Superblocks architecture, let's proceed with building our application. As an optional step, you can set up the On-Premise Agent (OPA) locally for secure database access. This local setup can be valuable for development and testing purposes.
