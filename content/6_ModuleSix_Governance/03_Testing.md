---
title: "Testing Access Controls"
chapter: true
weight: 3
---

Let's verify your access controls work correctly by testing with different user scenarios.

## Step 1: Test in Preview

1. Open Preview mode:

    - Click "Preview" in the top-right
    - This shows a preview of the dashboard and allows you to see how it would look for different users

2. Try different use cases:

    - Add your name to the rule to grant access to the Invoices tab
    - Check what you can see in the preview
    - Verify restrictions work


## Test Access Rules

1. Test restricted access:

    - Log in as yourself (if your name is not John Smith, Sarah Johnson, or Michael Lee)
    - Verify Invoices tab is hidden

2. Test with access:

    - Add your name to the rule:

   ```javascript
   {{['John Smith', 'Sarah Johnson', 'Michael Lee', 'Your Name'].includes(Global.user.name)}}
   ```

    - Verify Invoices tab appears


## Security Checklist

✓ User names display correctly
✓ Access rules work consistently
✓ UI updates properly
✓ Sensitive data is protected

{{% notice tip %}}
Test thoroughly! Try different users and edge cases to ensure your security is solid.
{{% /notice %}}

## Governance Module Complete

Excellent work! You've successfully implemented:

1. A modern dashboard frontend
2. Powerful backend APIs
3. AI features with Amazon Bedrock
4. Enterprise security controls
5. A fully tested application

Your Acme Incorporated dashboard now has proper governance and access controls in place. Next, we'll deploy your application to production!
