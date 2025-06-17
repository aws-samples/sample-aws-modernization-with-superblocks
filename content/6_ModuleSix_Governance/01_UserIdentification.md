---
title: "User Identification"
chapter: true
weight: 1
---

Let's display user information in your dashboard to help users identify who's logged in.

## Step 1: Set Up User Display
1. Find the user icon:
   - Look in the top-right corner
   - Click to select the icon component

2. Add the user's name:
   - Open the Properties panel
   - Find the "Label" field
   - Add this JavaScript:
   ```javascript
   {{Global.user.name}}
   ```

## Working with User Context

The `Global.user` object gives you access to:
- `name`: Full name
- `email`: Email address
- `groups`: Access groups
- `metadata`: Additional info

Example uses:
```javascript
// Display name
{{Global.user.name}}

// Show email
{{Global.user.email}}

// Check admin access
{{Global.user.groups.includes('admin')}}
```

{{% notice tip %}}
The `Global.user` object is always available and automatically stays in sync with the current user's session.
{{% /notice %}}

## Next Steps
Now that users can identify themselves, let's set up access controls.
