---
pcx_content_type: concept
title: Permissions
weight: 40
---

# Permissions

Below is a description of the available permissions for tokens and roles as they relate to Logs. For information about how to create an API token, refer to [Creating API tokens](/fundamentals/api/get-started/create-token/).

## Tokens

*   **Logs: Read** - Grants read access to logs using Logpull or Instant Logs.

*   **Logs: Write** - Grants read and write access to Logpull and Logpush, and read access to Instant Logs.

{{<Aside type="note" header="Note">}}
For zone scoped datasets, tokens must be zone scoped. For account scoped datasets, tokens must be account scoped.
{{</Aside>}}

## Roles

**Super Administrator**, **Administrator** and the **Log Share** roles have full access to Logpull, Logpush and Instant Logs. 

Only roles with **Log Share** edit permissions can read and configure Logpush jobs because job configurations may contain sensitive information.

The **Administrator Read only** and **Log Share Reader** roles only have access to Instant Logs and Logpull. This role does not have permissions to view the configuration of Logpush jobs.

### Assign or remove a role

To check the list of members in your account, or to manage roles and permissions:

1.  Navigate to the [Cloudflare dashboard](https://dash.cloudflare.com/login) and select your account.
2.  From your Account Home, go to **Manage Account** > **Members**.
3.  Enter a member’s email address to add them to your account, and select **Invite**.
4.  Alternatively, scroll down to the **Members** card to find a list of members with their status and role.

For more information, refer to [Managing roles within your Cloudflare account](/fundamentals/setup/manage-members/).