---
pcx_content_type: reference
title: Redirect one domain to another
---

# Redirect one domain to another

If you have an alias domain that only forwards traffic to another domain, you can set up redirects directly within Cloudflare.

1. [Add](/fundamentals/setup/manage-domains/#add-a-domain-to-cloudflare) your alias domain (for example, `previous.com`) to Cloudflare.

2. Make sure that your alias domain has a proxied [DNS A or CNAME record](/dns/manage-dns-records/how-to/create-dns-records/) that properly resolves DNS queries. You may also want to include a record for the `www` subdomain.

    | **Type** | **Name** | **IPv4 address** | **Proxy status** |
    | -------- | -------- | ---------------- | ---------------- |
    | A        | `@`      | `192.0.2.1`      | Proxied          |
    | A        | `www`    | `192.0.2.1`      | Proxied          |

3. Use [Redirect rules](/rules/url-forwarding/) to forward traffic from your alias domain to your other domain.

    {{<render file="url-forwarding/_different-hostname-example.md" productFolder="rules">}}