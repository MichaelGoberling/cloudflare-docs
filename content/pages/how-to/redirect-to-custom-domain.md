---
pcx_content_type: how-to
title: Redirecting *.pages.dev to a Custom Domain
---

# Redirect \*.pages.dev to a custom domain

Learn how to use [Bulk Redirects](/rules/url-forwarding/bulk-redirects/) to redirect your `*.pages.dev` subdomain to your [custom domain](/pages/configuration/custom-domains/).

You may want to do this to ensure that your site's content is served only on the custom domain, and not the `*.pages.dev` site automatically generated on your first Pages deployment.

## Setup

To redirect a `*.pages.dev` subdomain to your custom domain:

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/?to=/:account/pages/view/:pages-project/domains), and select your account.
2. Select **Workers & Pages** and select your Pages application.
3. Go to **Custom domains** and make sure that your custom domain is listed. If it is not, add it by clicking **Set up a custom domain**.
4. Go **Bulk Redirects**.
5. [Create a bulk redirect list](/rules/url-forwarding/bulk-redirects/create-dashboard/#1-create-a-bulk-redirect-list) modeled after the following (but replacing the values as appropriate):

  {{<example>}}

  | Source URL | Target URL  | Status | Parameters |
  | ---- | ----- | ------------ | ------------ |
  | `*.pages.dev`    | `https://example.com` | `301`  | <ul><li>Preserve query string</li><li>Subpath matching</li><li>Preserve path suffix</li><li>Include subdomains</li></ul> |

  {{</example>}}

6. [Create a bulk redirect rule](/rules/url-forwarding/bulk-redirects/create-dashboard/#2-create-a-bulk-redirect-rule) using the list you just created.

To test that your redirect worked, go to your `*.pages.dev` domain. If the URL is now set to your custom domain, then the rule has propagated.

## Related resources

- [Redirect www to domain apex](/pages/how-to/www-redirect/)
- [Handle redirects with Bulk Redirects](/rules/url-forwarding/bulk-redirects/)