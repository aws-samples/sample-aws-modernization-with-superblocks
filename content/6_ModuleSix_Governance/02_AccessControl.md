---
title: "Implementing Access Control"
chapter: true
weight: 2
---

Let's secure your dashboard by controlling who can access sensitive features.

## Step 1: Add Access Rules

1. Find the Invoices tab:

   - Locate "Invoices" in the navigation
   - Select to view its properties

2. Create visibility rule:

   - Find "Visibility" under Layout
   - Add this access check:

   ```javascript
   {{['John Smith', 'Sarah Johnson', 'Michael Lee'].includes(Global.user.name)}}
   ```

## How Access Control Works

This rule:

- Only shows Invoices to users in the finance group
- Hides it from everyone else
- Keeps the UI clean and secure
- Follows security best practices by using group-based access

## More Access Examples

Control access by group membership:

```javascript
// Manager-only features
{{Global.user.groups.includes('manager')}}

// Admin dashboard access
{{Global.user.groups.includes('admin')}}
```

## Security Best Practices

1. Secure both UI and APIs
2. Use consistent rules
3. Keep rules simple
4. Document permissions
5. Use groups instead of individual names

{{% notice tip %}}
It is always recommended to secure your APIs in addition to implementing UI controls. Groups are also easier to maintain, more scalable, and follow security best practices instead of individual names.
{{% /notice %}}

## Next Steps

Let's test your access controls to make sure they work.
