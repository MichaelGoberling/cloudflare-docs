---
pcx_content_type: reference
title: Application token
weight: 2
---
# Application token

Cloudflare Access includes the application token with all authenticated requests to your origin. A typical JWT looks like this:

`eyJhbGciOiJSUzI1NiIsImtpZCI6IjkzMzhhYmUxYmFmMmZlNDkyZjY0.eyJhdWQiOlsiOTdlMmFhZ TEyMDEyMWY5MDJkZjhiYzk5ZmMzNDU5MTNh.zLYsHmLEginAQUXdygQo08gLTExWNXsN4jBc6PKdB`

As shown above, the JWT contains three Base64-URL values separated by dots:

* [Header](#header)
* [Payload](#payload)
* [Signature](#signature)

Unless your application is connected to Access through Cloudflare Tunnel, your application must validate the token to ensure the security of your origin. Validation of the header alone is not sufficient — the JWT and signature must be confirmed to avoid identity spoofing.

## Header

```json
{
  "alg": "RS256",
  "kid": "9338abe1baf2fe492f646a736f25afbf7b025e35c627be4f60c414d4c73069b8",
  "typ": "JWT"
}
```

* `alg` identifies the encoding algorithm.
* `kid` identifies the key used to sign the token.
* `typ` designates the token format.

## Payload

```json
{
  "aud": [
    "32eafc7626e974616deaf0dc3ce63d7bcbed58a2731e84d06bc3cdf1b53c4228"
  ],
  "email": "user@example.com",
  "exp": 1659474457,
  "iat": 1659474397,
  "nbf": 1659474397,
  "iss": "https://yourteam.cloudflareaccess.com",
  "type": "app",
  "identity_nonce": "6ei69kawdKzMIAPF",
  "sub": "7335d417-61da-459d-899c-0a01c76a2f94",
  "country": "US"
}
```

The payload contains the actual claim and user information to pass to the application.

| Field           | Description                               |
|-----------------|-------------------------------------------|
| aud             | The audience tag for the application to which the token is issued. |
| email           | The email address of the authenticated user. |
| exp             | The expiration timestamp for the token. |
| iat             | The issuance timestamp for the token. |
| nbf             | The not-before timestamp for the token, used to checks if the token was received before it should be used.|
| iss             | The Cloudflare Access domain URL for the application.|
| type            | The type of Access token (`app` for application token or `org` for global session token).|
| identity_nonce  | A nonce used to get the [user's identity](#user-identity).|
| sub             | The ID of the user. |
| country         | The country where the user authenticated from. |

### Custom SAML attributes and OIDC claims

Access allows you to add custom SAML attributes and OIDC claims to your JWT for enhanced verification, if supported by your identity provider. This is configured when you setup your [SAML](/cloudflare-one/identity/idp-integration/generic-saml/) or [OIDC](/cloudflare-one/identity/idp-integration/generic-oidc/) provider.

### User identity

User identity is useful for checking application permissions. For example, your application can validate that a given user is a member of an Okta or AzureAD group such as `Finance-Team`.

Due to cookie size limits and bandwidth considerations, the application token only contains a subset of the user's identity. To get the user's full identity, send the `CF_Authorization` cookie to `https://<your-team-name>.cloudflareaccess.com/cdn-cgi/access/get-identity`. Your request should be structured as follows:

```sh
$ curl -H 'cookie: CF_Authorization=<user-token>' https://<your-team-name>.cloudflareaccess.com/cdn-cgi/access/get-identity
```

Access will return a JSON structure containing the following data:

| Field           | Description                               |
|-----------------|-------------------------------------------|
| email           | The email address of the user. |
| idp             | Data from your identity provider. |
| geo             | The country where the user authenticated from. |
| user_uuid       | The ID of the user. |
| devicePosture   | The device posture attributes. |
| account_id      | The account ID for your organization. |
| iat             | The timestamp indicating when the user logged in. |
| ip              | The IP address of the user. |
| auth_status     | The status if authenticating with mTLS. |
| common_name     | The common name on the mTLS client certificate. |
| service_token_id | The ID of the service token used for authentication. |
| service_token_status | True if authentication was through a service token instead of an IdP. |
| is_warp         | True if the user enabled WARP. |
| is_gateway      | True if the user enabled WARP and authenticated to a Zero Trust team. |
| gateway_account_id | An ID generated by the WARP client when authenticated to a Zero Trust team.|
| device_id       | The ID of the device used for authentication. |
| version         | The version of the `get-identity` object. |
| device_sessions | A list of all sessions initiated by the user. |

## Signature

Cloudflare generates the signature by signing the encoded header and payload using the SHA-256 algorithm (RS256). In RS256, a private key signs the JWTs and a separate [public key](/cloudflare-one/identity/authorization-cookie/validating-json/#access-signing-keys) verifies the signature.

For more information on JWTs, refer to [jwt.io](https://jwt.io/).