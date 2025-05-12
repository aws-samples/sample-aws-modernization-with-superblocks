---
title: "Implementing Access Control"
chapter: true
weight: 2
---

# Implementing Access Control

In this section, we'll implement role-based access control for different features of our dashboard.

## Configure Access Rules

1. Navigate to "Invoices" Tab
   - Find the "Invoices" button in the navigation bar
   - Access its properties in the Properties panel

2. Set Visibility Rule
   - Locate the "Visibility" property under "Layout"
   - Add the following JavaScript:
   ```javascript
   {{['Oscar Martinez', 'Angela Schrute', 'Kevin Mallone'].includes(Global.user.name) ? true : false}}
   ```

## Understanding Access Control

This implementation:
- Restricts "Invoices" access to specific users
- Hides the button completely for unauthorized users
- Maintains a clean UI for all users

## Additional Access Controls

You can implement similar controls for:

1. Data Visibility
```javascript
// Show sensitive data only to managers
{{Global.user.roles.includes('manager') ? sensitiveData : '***'}}
```

2. Feature Access
```javascript
// Enable editing for admin users
{{Global.user.roles.includes('admin')}}
```

3. Action Permissions
```javascript
// Allow delete operations for specific roles
{{['admin', 'data_manager'].some(role => Global.user.roles.includes(role))}}
```

## Best Practices

1. Always implement access control at both UI and API levels
2. Use consistent access patterns across your application
3. Keep access rules simple and maintainable
4. Document access requirements clearly

{{% notice warning %}}
UI-level access control is not sufficient on its own. Always implement corresponding backend access controls in your APIs.
{{% /notice %}}

## Testing Access Control

Test with different user accounts:
1. Accounting team members (Oscar, Angela, Kevin)
   - Should see Invoices tab
   - Can access financial data

2. Other users
   - Should not see Invoices tab
   - Cannot access restricted features

## Next Steps
In the final section, we'll test our access control implementation.
