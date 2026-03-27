---
title: 'Step 3: Send an SMS'
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: Read more about our API Docs
  pages:
    - type: endpoint
      slug: post_v2-sms
      title: Send SMS
---
Let's send your first SMS message with your newly minted API key and API secret! The simplest way is to send the request through the [Mac Terminal](https://macpaw.com/how-to/use-terminal-on-mac) or [Windows PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.4) .

## Part 1.) Create your Basic Authentication Token

The Authorization token is a Base64 encoded string, combining your `API_KEY` + ":" + "`API_Secret` 

### Mac Terminal

* `echo -n "API_KEY:API_Secret" | base64`

### Windows PowerShell

```powershell
$myString = "API_KEY:API_Secret"
$base64String = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($myString))
echo $base64String
```

## Part 2.) Send the message

> 🚧 Kudosity trial accounts can only send messages to the mobile number provided during account sign up.

Update the code with your encoded authentication token and the number you are trying to send to:

* `Authorization:` Basic **XXXXXXXX**
* `To` - **XXXXXXX** - Kudosity trial accounts can only send messages to the number registered for their account. 

**Mac Terminal**

```Text CURL
curl --location 'https://api.transmitsms.com/send-sms.json' \
--header 'Authorization: Basic XXXXXXXX' \
--form 'message="Hello, World"' \
--form 'to="XXXXXXXXXX"'
```

**Windows PowerShell**

```powershell
# Define the API endpoint
$apiUrl = "https://api.transmitsms.com/send-sms.json"

# Define the headers
$headers = @{
    "Authorization" = "Basic " + $base64String
    "Content-Type"  = "application/x-www-form-urlencoded"
}

# Define the Body
$form = @{
    message = "Hello, World"
    to = "XXXXXXX"
}

#Convert body data to JSON
$formEncoded = ($form.GetEnumerator() | ForEach-Object { [System.Uri]::EscapeDataString($_.Key) + "=" + [System.Uri]::EscapeDataString($_.Value) }) -join "&"

$response = Invoke-RestMethod -Uri $apiUrl -Method Post -Body $formEncoded -Headers $headers -ContentType "application/x-www-form-urlencoded"

# Display the response
echo $response
```

## Response

```json
{
  "message_id": 1141834139,
  "send_at": "2024-05-09 02:29:33",
  "recipients": 1,
  "cost": 0.087,
  "sms": 1,
  "delivery_stats": {
    "delivered": 0,
    "pending": 0,
    "bounced": 0,
    "responses": 0,
    "optouts": 0
  },
  "message_overidden": "Your message has been overridden by this pre configured demo message during the trial period",
  "error": {
    "code": "SUCCESS",
    "description": "OK"
  }
}
```

Congratulations! You've just sent your first message through the Kudosity API :grinning: :rocket:

As part of our trial experience, we'll override the message and send a templated messages.
