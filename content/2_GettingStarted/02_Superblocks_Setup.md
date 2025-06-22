---
title: "Superblocks Setup"
chapter: true
weight: 4
---

# Resource Setup

## Prerequisites

To complete this workshop, you'll need:

1. A Superblocks Account
2. Database Access
3. A Modern Web Browser (Chrome recommended)

## Step 1: Superblocks Account Setup

**Create a Superblocks Account**
- Go to [superblocks.com](https://www.superblocks.com)
- Click "Login" then "Sign up"
- Use your work email (note: public domains like gmail.com are not allowed)
- Verify your email address

**Already have an account?**
- Go to [superblocks.com](https://www.superblocks.com)
- Click "Login"
- Enter your credentials

## Step 2: Database Configuration
{{% notice info %}}
If you're attending a guided workshop, you'll be provided with database credentials and can skip the RDS setup section below.
{{% /notice %}}

**1. Locate Your Database Credentials**

If you're using the workshop environment, your database credentials are already set as environment variables. You can view them by running:

```bash
# Display your database credentials
echo "Database Host: $DB_ENDPOINT"
echo "Database Name: $DB_NAME"
echo "Database Username: $DB_USERNAME"
echo "Database Password: $DB_PASSWORD"
```

Copy these values as you'll need them for the next step.

**Configure Superblocks Connection**

1. In Superblocks, go to [Integrations](https://app.superblocks.com/integrations)
2. Click "Add Integration"
3. Search for and select "PostgreSQL"
4. Enter your database details:

   - Integration Name: `aws-superblocks-rds`
   - Connection method: Form
   - Host: The DB_ENDPOINT value from the previous step
   - Port: 5432 (default for Postgres)
   - Database: The DB_NAME value from the previous step
   - Username: The DB_USERNAME value from the previous step
   - Password: The DB_PASSWORD value from the previous step

5. Click "Test Connection" to verify and click "Create"

{{% notice warning %}}
If you can't connect to the database or access any part of Superblocks, please ask for assistance or reach out to Superblocks support before proceeding.
{{% /notice %}}

## Next Steps

After completing the setup, we'll explore the technical concepts behind our application.
