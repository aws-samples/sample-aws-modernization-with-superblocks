---
title: "Testing Access Controls"
chapter: true
weight: 3
---

# Testing Access Controls

In this final section, we'll test our access control implementation to ensure it works as expected.

## Preview Mode Testing

1. Access Preview Mode
   - Click the "Preview" button in the top right
   - This simulates how different users will see the application

2. Test Different User Scenarios
   - Log in as different users
   - Verify feature visibility changes
   - Confirm access restrictions work

## Test Cases

### Accounting Team Access

Test as "Oscar Martinez":
1. Verify Invoices tab is visible
2. Confirm access to financial data
3. Check all accounting features work

Test as "Regular User":
1. Confirm Invoices tab is hidden
2. Verify restricted features are not accessible
3. Ensure basic functionality works

## Common Issues and Solutions

1. Visibility Issues
   ```javascript
   // Problem: Inconsistent visibility
   {{Global.user.name === 'Oscar Martinez'}}

   // Better approach
   {{['Oscar Martinez', 'Angela Schrute', 'Kevin Mallone'].includes(Global.user.name)}}
   ```

2. Role Checking
   ```javascript
   // Problem: Single role check
   {{Global.user.roles.includes('admin')}}

   // Better approach: Multiple roles
   {{['admin', 'manager'].some(role => Global.user.roles.includes(role))}}
   ```

## Validation Checklist

✓ User identification works correctly
✓ Access controls are consistent
✓ UI elements hide/show appropriately
✓ Error messages are clear
✓ No unauthorized access possible

{{% notice tip %}}
Always test with multiple user accounts to ensure access controls work consistently across different scenarios.
{{% /notice %}}

## Workshop Completion

Congratulations! You have now:
1. Built a complete dashboard frontend
2. Implemented backend APIs
3. Set up proper access controls
4. Tested the entire application

Your [PLACEHOLDER ]dashboard is now ready for production use!
