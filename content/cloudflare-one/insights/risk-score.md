---
pcx_content_type: concept
title: Risk score
weight: 4
meta:
    title: User risk score
---

# Risk score

{{<Aside type="note">}}
Only available on Enterprise plans.
{{</Aside>}}

Zero Trust risk scoring detects user activity and behaviors that could introduce risk to your organization’s systems and data. Risk scores add user and entity behavior analytics (UEBA) to the Zero Trust platform.

## User risk scoring

Cloudflare Zero Trust assigns a risk score of Low, Medium, or High based on detections of users’ activities, posture, and settings. A user’s score is equal to the highest-level risk behavior they trigger.

### View a user's risk score

To view a user's risk score in [Zero Trust](https://one.dash.cloudflare.com/), go to **Risk score** > **User risk scoring**. Select a user's name to view their instances of risk behaviors, if any. You can select an instance of a risk behavior to view the log associated with the detection.

Users that have had their risk score [cleared](#clear-a-users-risk-score) will not appear in the table unless they trigger another risk behavior.

### Clear a user’s risk score

If required, you can reset risk scores for specific users. Once reset, users will not appear in the associated risk table until they trigger another risk behavior.

1. In [Zero Trust](https://one.dash.cloudflare.com/), go to **Risk score** > **User risk scoring**.
2. Select the user you want to clear the risk score for.
3. In **User risk overview**, select **Reset user risk**.
4. Select **Confirm**.

### Send risk score to Okta

In addition to controls in Zero Trust, Okta users can send risk scores to Okta to apply SSO-level policies.

First, configure Zero Trust to send user risk scores to Okta.

   1. Set up the [Okta SSO integration](/cloudflare-one/identity/idp-integration/okta/).
   2. In [Zero Trust](https://one.dash.cloudflare.com/), go to **Settings** > **Authentication**.
   3. In **Login methods**, locate your Okta integration and select **Edit**.
   4. Turn on **Send risk score to Okta**.
   5. Select **Save**.
   6. Upon saving, Zero Trust will display the well-known URL for your organization. Copy the value.

Next, configure Okta to receive your risk scores.

   1. On your Okta admin dashboard, go to **Security** > **Device Integrations**.
   2. Go to **Receive shared signals**, then select **Create stream**.
   3. Name your integration. In **Set up integration with**, choose _Well-known URL_.
   4. In **Well-known URL**, enter the well-known URL value provided by Zero Trust.
   5. Select **Create**.

For more information on configuring user risk score within Okta, refer to the [Okta documentation](https://help.okta.com/oie/en-us/content/topics/itp/overview.htm).

While the Okta integration is turned on, Zero Trust will send any user risk score updates to Okta, including score increases and resets. Score update events will appear in your [Access audit logs](/cloudflare-one/insights/logs/audit-logs/).

## Predefined risk behaviors

By default, all predefined behaviors are disabled. When a behavior is enabled, Zero Trust will continuously evaluate all users within the organization for the behavior. You can [change the risk level](#change-risk-behavior-risk-levels) for predefined behaviors if the default assignment does not suit your environment.

| Risk behaviors                         | Requirements                                                                                                | Description                                                                                                                                                                                                            |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Impossible travel                      | [A configured Access application](/cloudflare-one/applications/)                                            | User has a successful login from two different locations that they could not have traveled between in that period of time. Matches will appear in your [Access audit logs](/cloudflare-one/insights/logs/audit-logs/). |
| High number of DLP policies triggered  | [A configured DLP profile](/cloudflare-one/policies/data-loss-prevention/dlp-profiles/)                     | User has created a high number of DLP policy matches within a narrow frame of time. Matches will appear in your [Gateway activity logs](/cloudflare-one/insights/logs/gateway-logs/).                                  |
| SentinelOne threat detected on machine | [SentinelOne service provider integration](/cloudflare-one/identity/devices/service-providers/sentinelone/) | SentinelOne returns one or more configured [device posture attributes](/cloudflare-one/identity/devices/service-providers/sentinelone/#device-posture-attributes) for a user.                                          |

## Manage risk behaviors

To toggle risk behaviors, go to **Risk score** > **Risk behaviors**.

### Enable risk behaviors

When a specific behavior is enabled, Zero Trust will continuously monitor all users within the organization for any instances of that behavior.

If a user engages in an enabled risk behavior, their risk level is re-evaluated. Zero Trust will update their risk score to the highest value between the current risk level and the risk level of the behavior they triggered.

### Disable risk behaviors

When a risk behavior is disabled, monitoring for future activity will cease. Previously detected risk behaviors will remain in the logs and associated with a user.

### Change risk behavior risk levels

You can change the risk level for a behavior at any time.

1. In [Zero Trust](https://one.dash.cloudflare.com/), go to **Risk score** > **Risk behaviors**.
2. Select the risk behavior you want to modify.
3. In the drop-down menu, choose your desired risk level.
4. Select **Save**.