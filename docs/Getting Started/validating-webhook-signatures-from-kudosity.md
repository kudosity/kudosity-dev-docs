---
title: Validating Webhook Signatures from Kudosity
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    The document explains how to verify webhooks from Kudosity by using an
    `x-transmitsms-signature` header, which contains an SHA-256 hash created
    with callback parameters and an API secret, and provides a PHP script to
    demonstrate the hash construction process for securing web applications.
  keywords:
    - webhooks
    - ' dlr'
    - ' kudosity'
    - ' sms integration'
  robots: index
next:
  description: ''
---
Your web application should verify that Kudosity is the service that sent a webhook before responding to that request. This is important for securing sensitive data, and to protect your application and servers from abuse.

Kudosity will sign callbacks to your application with an `x-transmitsms-signature` HTTP header.

### Header Details:

The `x-transmitsms-signature` header contains an SHA-256 hash, created using the parameters returned in the callback and the account’s API secret as the encryption salt.

### Test PHP Script

Below is a sample PHP script that demonstrates how the hash is constructed, enabling customers to replicate the process in their applications. This script can serve as a guide for building a module to verify the `x-transmitsms-signature` against query string parameters and the API secret, and thereby validate and secure your webhooks.

```Text PHP
<?php
$params = [
  'message_id' => 'example_message_id',
  'mobile' => 'example_mobile_number',
  'datetime' => 'example_datetime',
  'status' => 'delivered',
  'user_id' => 'example_user_id',
  'rate' => 10,
];

$builturl = "https://example-url.com";
$builturl .= (strpos($builturl, '?') ? '&' : '?') . http_build_query($params);
$secret = 'your_api_secret';

echo "URL: {$builturl}\n";
$query = parse_url($builturl, PHP_URL_QUERY);
parse_str($query, $hashparams);
$json = json_encode($hashparams);

echo "JSON: {$json}\n";
$hash = hash_hmac('sha256', $json, $secret);
echo "Generated Hash: {$hash}\n";
?>
```

Note:\
When generating the hash, ensure that parameters are in the exact order they appear in the callback. Any deviation in parameter order will result in a different hash.

Additionally, if using tools like Webhook.site, be aware that such tools may reorder parameters before displaying results, which can cause discrepancies in the generated hash.

#### Usage Instructions:

1. Replace the placeholder values in `$params` with the actual query string parameters returned in the callback.
2. Set the `$builturl` to the webhook’s target URL.
3. Set `$secret` to the API secret found in the account settings.

<br />

## Sample Delivery Receipt (DLR) Callback:

Example Send

<Image align="center" className="border" border={true} src="https://files.readme.io/365d90a4ceb7c41bc97539f7781b7dfba65b4c0a1d64ea88c9e1c59f84023ab9-Screenshot_2024-11-12_at_4.11.10_pm.png" />

Captured DLR callback on Webhook.site

<Image align="center" className="border" border={true} src="https://files.readme.io/949842c9c5b80fe03eaa41bc1334fdb507d6548bdd26e87f0bea080e9d787c3a-Screenshot_2024-11-12_at_4.12.31_pm.png" />

Sample Output

`x-transmitsms-signature`: 27b6822a9014c87da728cd510d51c7731464f59274fddc4e14959df712956f72

```Text PHP
URL: https://webhook.site/fdf0e2c2-c4bb-42d4-8405-0ab90e16aec9?message_id=483979011&mobile=XXXXXXX&datetime=2021-04-26+04%3A18%3A00&status=delivered&user_id=XXXXXXX&rate=10
array(6) {
  ["message_id"]=>
  string(9) "483979011"
  ["mobile"]=>
  string(11) "61429859724"
  ["datetime"]=>
  string(19) "2021-04-26 04:18:00"
  ["status"]=>
  string(9) "delivered"
  ["user_id"]=>
  string(5) "63935"
  ["rate"]=>
  string(2) "10"
}
JSON: {"message_id":"483979011","mobile":"XXXXXXXXX","datetime":"2021-04-26 04:18:00","status":"delivered","user_id":"XXXXXX","rate":"10"}
Built: 27b6822a9014c87da728cd510d51c7731464f59274fddc4e14959df712956f72
```
