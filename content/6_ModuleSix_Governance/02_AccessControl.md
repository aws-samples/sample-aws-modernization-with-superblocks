---
title: "Implementing Access Control"
chapter: true
weight: 2
---

Let's secure your dashboard by controlling who can access sensitive features.

## Step 1: Add Access Rules

1. Find the Invoices tab and create a visibility rule:

   - Select "Invoices" in the navigation header to view its properties
   - In the Layout section, find the "Visibility" property
   - Select the <> icon next to "Visibility"
   - Add the below access check:

   ```sh
   {{["John Smith", "Sarah Johnson", "Michael Lee"].includes(Global.user.name)}}
   ```

## How Access Control Works

This rule:

- Only shows Invoices to users whose name matches the provided values
- Hides it from everyone else
- Keeps the UI clean and secure

## More Access Examples

Control access by group membership:

```sh
// Manager-only features
{{Global.user.groups.includes("manager")}}

// Admin dashboard access
{{Global.user.groups.includes("admin")}}
```

## Security Best Practices

1. Secure both UI and APIs
2. Use consistent rules
3. Keep rules simple
4. Document permissions

{{% notice tip %}}
It is always recommended to secure your APIs in addition to implementing UI controls. Groups are also easier to maintain and more scalable.
{{% /notice %}}

## Next Steps

Let's test your access controls to make sure they work.
