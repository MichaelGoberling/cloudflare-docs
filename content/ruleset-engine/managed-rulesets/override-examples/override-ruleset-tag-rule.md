---
pcx_content_type: configuration
title: Deploy a managed ruleset with ruleset, tag, and rule overrides
weight: 5
---

# Deploy a managed ruleset with ruleset, tag, and rule overrides

Customize the execution of managed rulesets with a combination of ruleset overrides, tag overrides, and rule overrides in your phase entry point ruleset.

1. [Add a rule](/ruleset-engine/basic-operations/deploy-rulesets/) to a phase entry point ruleset to execute a managed ruleset.
2. [Configure a ruleset override](/ruleset-engine/managed-rulesets/override-managed-ruleset/) that disables all rules in the managed ruleset.
3. [Configure a tag override](/ruleset-engine/managed-rulesets/override-managed-ruleset/) that sets an action for rules with a given tag.
4. [Configure a rule override](/ruleset-engine/managed-rulesets/override-managed-ruleset/) that sets an action for the rules you want to execute.

The request below uses the [Update ruleset](/ruleset-engine/rulesets-api/update/) operation to execute the following in a single `PUT` request:

* Add a rule to the `http_request_firewall_managed` phase entry point ruleset that executes a managed ruleset.
* Use category overrides to enable rules with `wordpress` and `drupal` tags and set their actions to `log`.
* Add a rule override that enables a single rule.

{{<details header="Example: Execute a managed ruleset at the zone level with overrides">}}

In this example:

* `"id": "<MANAGED_RULESET_ID>"` adds a rule to the `http_request_firewall_managed` phase entry point ruleset to execute a managed ruleset for requests addressed to a zone (`{zone_id}`).
* `"enabled": false` defines an override at the ruleset level to disable all rules in the managed ruleset.
* `"categories": [{"category": "wordpress", "action": "log", "enabled": true}, {"category": "drupal", "action": "log", "enabled": true}]` defines an override at the tag level to enable rules tagged with `wordpress` or `drupal` and sets their action to `log`.
* `"rules": [{"id": "<RULE_ID>", "action": "block", "enabled": true}]` defines an override at the rule level that enables one individual rule and sets the action to `block`.

```bash
curl --request PUT \
https://api.cloudflare.com/client/v4/zones/{zone_id}/rulesets/phases/http_request_firewall_managed/entrypoint \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '{
  "rules": [
    {
      "action": "execute",
      "expression": "true",
      "action_parameters": {
        "id": "<MANAGED_RULESET_ID>",
        "overrides": {
          "enabled": false,
          "categories": [
            {
              "category": "wordpress",
              "action": "log",
              "enabled": true
            },
            {
              "category": "drupal",
              "action": "log",
              "enabled": true
            }
          ],
          "rules": [
            {
              "id": "<RULE_ID>",
              "action": "block",
              "enabled": true
            }
          ]
        }
      }
    }
  ]
}'
```

{{</details>}}

{{<details header="Example: Execute a managed ruleset at the account level with overrides">}}

In this example:

* `"id": "<MANAGED_RULESET_ID>"` adds a rule to the `http_request_firewall_managed` phase entry point ruleset that executes a managed ruleset for requests addressed to `example.com`.
* `"enabled": false` defines an override at the ruleset level to disable all rules in the managed ruleset.
* `"categories": [{"category": "wordpress", "action": "log", "enabled": true}, {"category": "drupal", "action": "log", "enabled": true}]` defines an override at the tag level to enable rules tagged with `wordpress` or `drupal` and sets their action to `log`.
* `"rules": [{"id": "<RULE_ID>", "action": "block", "enabled": true}]` defines an override at the rule level that enables one individual rule and sets the action to `block`.

```bash
curl --request PUT \
https://api.cloudflare.com/client/v4/accounts/{account_id}/rulesets/phases/http_request_firewall_managed/entrypoint \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '{
  "rules": [
    {
      "action": "execute",
      "expression": "cf.zone.name eq \"example.com\" and cf.zone.plan eq \"ENT\"",
      "action_parameters": {
        "id": "<MANAGED_RULESET_ID>",
        "overrides": {
          "enabled": false,
          "categories": [
            {
              "category": "wordpress",
              "action": "log",
              "enabled": true
            },
            {
              "category": "drupal",
              "action": "log",
              "enabled": true
            }
          ],
          "rules": [
            {
              "id": "<RULE_ID>",
              "action": "block",
              "enabled": true
            }
          ]
        }
      }
    }
  ]
}'
```

{{</details>}}