# Webhook

## Setup

You can easily setup and manage webhooks in ImageKit dashboard.

Navigate to [dashboard/developer/webhooks](https://imagekit.io/dashboard/developer/webhooks)

### Creating a webhook

- Click `+ Add webhook` button
- A modal will appear
- Enter webhook endpoint URL
- Click `Create` button

### Deleting a webhook

- Click on options icon next to the webhook
- Click `Delete` button
- A dialog will appear asking you to confirm deletion of the webhook
- Click `Delete` button

### Enable/Disable a webhook

- Click on options icon next to the webhook
- Click on `Edit` button
- A modal will appear
- Switch `Enabled` checkbox to `On` if you want to enable the webhook or `Off` if you want to disable the webhook
- Click `Save` button

## Handling events

Imagekit will submit HTTP(S) POST to configured webhook URL, with a JSON body.

### Request body

All webhook bodies are JSON encoded. The schema of the body may differ based on the event type, but the following fields are identical in all.

| Field         | DataType | Description                              |
| ------------- | -------- | ---------------------------------------- |
| **type**      | `string` | Type of event                            |
| **id**        | `string` | Unique identifier of the event           |
| **createdAt** | `string` | Timestamp of the event as ISO8601 string |

### Response

Respond with `200 OK` to acknowledge receipt of the event.

Incase acknowledgement is not received, ImageKit will retry calling the webhook endpoint for the event.

### Security

ImageKit sends `x-ik-signature` in the webhook request header. This a SHA1 hash of the body, salted with webhook secret. This sign is used to verify the authenticity of the webhook request.

#### Webhook secret

Webhook secret is generated when you create a webhook in ImageKit dashboard.

To view the webhook secret, navigate to [dashboard/developer/webhooks](https://imagekit.io/dashboard/developer/webhooks).

#### Signature

Webhook signature comprises of two parts:

1. Timestamp: 8bytes of Unix timestamp in seconds since epoch.
2. Hash: SHA1 hash of the payload, salted with timstamp & webhook secret.

For example:

```txt
t:1655700962963,p_t_sha1:PTvifamM1cKkipHVjM0Bu1LFy6A=
```

- `t`: timestamp of the event in milliseconds since epoch.
- `p_t_sha1`: SHA1 hash of the payload, salted with timestamp & webhook secret.

Generating SHA1 hash

```js
const payload // Raw webhook payload in bytes
const secret // Webhook secret in bytes (decoded from base64 string)

const timestamp = Date.now() // in seconds since epoch as uint32
const timestampAsLEBytes = u64ToLeBytes(timestamp) // in little endian bytes

const hash = sha1([...payload, ...timestampAsLEBytes, ...secret]) // in bytes
```

#### Preventing replay attacks

Imagekit webhook request body contains UUID as a unique identifier of the event, along with the timestamp of the event. (`id` and `createdAt` fields in the webhook body).

This UUID is used to prevent replay attacks.

We may not map UUID for very long time, therefore we need to set expiration time for the UUID and discard the UUID after the expiration time. While we must stall the request if the timestamp of the event is older than the expiration time (ie `verifiedSignature.timestamp < Date.now() - expriryDuration`).

How much should be expiration time?

Webhook timestamp is generate just before calling the webhook endpoint. Therefore expiration time of 30 seconds is a good choice for most use cases, however 1min is more than sufficient in worst case scenarios.

### Usage Guide

You can use ImageKit SDK to verify the signature.

Imagekit NodeJS SDK in express.js server.

```ts
import express, { Request, Response } from "express";
import { WebhookSignature } from "imagekit";

const app = express();

const WEBHOOK_SECRET = "secret";
const WEBHOOK_EXPIRY_DURATION = 60 * 1000; // 60 seconds

app.post("/webhook", express.raw({ type: "application/json" }), (req: Request, res: Response) => {
    const signature = req.headers["x-ik-signature"] as string;
    const rawBody = req.body;

    let webhookResult;
    try {
        webhookResult = WebhookSignature.verify(rawBody, signature, WEBHOOK_SECRET);
    } catch (e) {
        // Failed to verify webhook
        return res.status(404).send(`Webhook error: ${e.message}`);
    }

    const { timestamp, event } = webhookResult;

    if ((timestamp + WEBHOOK_EXPIRY_DURATION) > Date.now()) {
        // Stall webhook
        return res.status(303).send(`Webhook expiry: ${timestamp}`);
    }

    // Handle webhook
    switch (event.type) {
        case "video.transformation.accepted": console.log("Video transformation request accepted"); break;
        case "video.transformation.ready": console.log("Video transformation ready"); break;
        case "video.transformation.error": console.log("Video transformation error"); break;
    }

    res.status(200).end(); // Acknowledge webhook is received and processed successfully
});
```
