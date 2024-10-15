---
_build:
  publishResources: false
  render: never
  list: never
inputParameters: productNamePath
---

[Magic Transit On Demand](/magic-transit/on-demand/) customers can use $1 to analyze their network traffic and detect DDoS attacks while Magic Transit is disabled. If an attack is detected, customers can automatically or manually enable Magic Transit to mitigate DDoS attacks.

Customers can create Magic Network Monitoring rules which will monitor specific IP {{<glossary-tooltip term_id="prefix">}}prefixes{{</glossary-tooltip>}} for DDoS attacks. When a DDoS attack is detected, Cloudflare  will notify you by email, [webhook](/notifications/get-started/configure-webhooks/), or [PagerDuty](/notifications/get-started/configure-pagerduty/) with information about the attack. Then, you can choose to [automatically activate IP advertisement](#activate-ip-auto-advertisement) and enable Magic Transit to protect the targeted IP prefixes from DDoS attacks. This feature is referred to as auto-advertisement, and you can enable it for individual Magic Network Monitoring rules via the dashboard or API.

After Magic Transit is activated and your traffic is flowing through Cloudflare, malicious DDoS traffic will be blocked, and your origin servers will only receive clean network traffic via IPsec or GRE tunnels.

The diagrams below illustrate this process:

<div class="large-img centered">

![The diagram shows the flow of traffic when you send flow data from your network to Cloudflare for analysis.](/images/magic-network-monitoring/1-flowdata.png)

</div>

<div class="large-img centered">

![Cloudflare automatically notifies you when we detect an attack based on your flow data.](/images/magic-network-monitoring/2-flowdata.png)

</div>

<div class="large-img centered">

![You can create rules to activate Magic Transit automatically, to protect your IP addresses from a DDoS attack.](/images/magic-network-monitoring/3-flowdata.png)

</div>

## Activate IP auto-advertisement

You need to enable IP auto-advertisement in order to use Magic Network Monitoring rules. You can activate IP auto-advertisement via the dashboard or the API.

### Dashboard

To activate IP advertisement via the Cloudflare dashboard, refer to [Configure dynamic advertisement](/byoip/concepts/dynamic-advertisement/best-practices/#configure-dynamic-advertisement).

### API

To activate IP advertisement via the API, refer to the [IP Address Management Dynamic Advertisement API](/api/operations/ip-address-management-dynamic-advertisement-get-advertisement-status).

## Magic Network Monitoring rules

To create Magic Network Monitoring rules with auto-advertisement, refer to [Rule Auto-Advertisement](/magic-network-monitoring/rules/#rule-auto-advertisement).