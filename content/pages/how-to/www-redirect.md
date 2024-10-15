---
pcx_content_type: how-to
title: Redirecting www to domain apex
---

# Redirecting www to domain apex

Learn how to redirect a `www` subdomain to your apex domain (`example.com`).

This setup assumes that you already have a [custom domain](/pages/configuration/custom-domains/) attached to your Pages project.

## Setup

To redirect your `www` subdomain to your domain apex:

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com) and select your account.
2. Go to **Bulk Redirects**.
3. [Create a bulk redirect list](/rules/url-forwarding/bulk-redirects/create-dashboard/#1-create-a-bulk-redirect-list) modeled after the following (but replacing the values as appropriate):

  {{<example>}}

  | Source URL | Target URL  | Status | Parameters |
  | ---- | ----- | ------------ | ------------ |
  | `www.example.com`    | `https://example.com` | `301`  | <ul><li>Preserve query string</li><li>Subpath matching</li><li>Preserve path suffix</li><li>Include subdomains</li></ul> |

  {{</example>}}

4. [Create a bulk redirect rule](/rules/url-forwarding/bulk-redirects/create-dashboard/#2-create-a-bulk-redirect-rule) using the list you just created.
5. Go to **DNS**.
6. [Create a DNS record](/dns/manage-dns-records/how-to/create-dns-records/#create-dns-records) for the `www` subdomain using the following values:

  {{<example>}}

  | Type | Name  | IPv4 address | Proxy status |
  | ---- | ----- | ------------ | ------------ |
  | `A`    | `www` | `192.0.2.1`  | Proxied      |

  {{</example>}}

It may take a moment for this DNS change to propagate, but once complete, you can run the following command in your terminal.

```sh
$ curl --head -i https://www.example.com/
```

Then, inspect the output to verify that the `location` header and status code are being set as configured.

## Related resources

- [Redirect `*.pages.dev` to a custom domain](/pages/how-to/redirect-to-custom-domain/)
- [Handle redirects with Bulk Redirects](/rules/url-forwarding/bulk-redirects/)