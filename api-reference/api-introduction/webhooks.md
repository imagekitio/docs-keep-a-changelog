# Webhooks

Imagekit uses webhooks to notify your application when an event happens in your account. Webhooks are particularly useful for asynchronous events such as video encoding and extension processing during upload.

## Steps to receive webhooks

1. Configure the webhook in your ImageKit dashboard.
2. Create a webhook endpoint as an HTTP endpoint (URL) on your server.
3. Handle requests from ImageKit by parsing each event object and returning `2xx` response status codes.

## How to configure a Webhook?

Go to [developer options](https://imagekit.io/dashboard/developer) in ImageKit dashboard. Under Webhooks, you will see the list of webhook endpoints configured.

Click on the "Add new" button to create a new webhook endpoint.

![Create new webhook endpoint](../../.gitbook/assets/add-new-webhook.png)

Enter a valid HTTP(S) endpoint and click on "Create".

![Create webhook form](../../.gitbook/assets/webhook-form.png)

You should see the webhook endpoint in the list now.

![Webhook endpoint lists](../../.gitbook/assets/webhook-list.png)

To update an existing endpoint, click on `...` and change the status.

![Update webhook endpoint](../../.gitbook/assets/disable-webhook.png)

## Listen to Webhook

Use a tool like [Ngrok](https://ngrok.com/) to make your local server accessible from outside for testing webhook implementation.

All webhook bodies are JSON encoded. The schema of the body may differ based on the event type, but the following fields are common.

| Field     | DataType | Description                              |
|-----------|----------|------------------------------------------|
| type      | `string` | Type of event.                           |
| id        | `string` | Unique identifier of the event.          |
| createdAt | `string` | Timestamp of the event in ISO8601 format.|
| data      | `JSON`   | Actual payload of event in JSON format.  |

## Verify webhook signature

Webhook endpoints are publicly accessible, therefore is important filter out malicious requests. We recommend that you use webhook signature to verify the authenticity of the webhook request.

To achieve this, every imagekit webhook endpoint comes with a HMAC signature in the request header `x-ik-signature`. To generate this signature, webhook secret is required, you can find webhook secret in [Imagekit dashboard](https://imagekit.io/dashboard/developer/webhooks).

> **Note:** Webhook secret should only be known to the server that is handling the webhook request.

### Verify signature with imagekit SDK

You can use imagekit SDK to verify & parse webhook request.

{% tabs %}
{% tab title="NodeJS SDK" %}

```js
const { Webhook } = require('imagekit');

function handler(req, res) {
  const signature = req.headers["x-ik-signature"];
  const rawBody = req.rawBody; // Raw request body encoded as Uint8Array or UTF-8 string

  const { timestamp, event } = Webhook.verify(rawBody, signature, WEBHOOK_SECRET);

  // Check if timestamp is within the tolerance limit

  // Handle event...

  // If everything is ok, respond with status code 2xx
  res.status(200).end();
}
```

{% endtab %}
{% endtabs %}

### Verify signature manually

Imagekit webhook signature follows the following format:

```js
t=${UNIX_TIMSTAMP_IN_MILLISECONDS},v1=${HMAC_SIGNATURE}
```

- The timestamp of signature is a Unix timestamp in milliseconds, prefixed with `t=`.
- The HMAC hash is prefixed with `v1=`.

Example of webhook signature

```txt
t=1655795539264,v1=b6bc2aa82491c32f1cbef0eb52b7ffaa51467ea65a03b5d4ccdcfb9e0941c946
```

Once you have retrived webhook signature from request header & raw request body, you can verify the authenticity of the webhook request in follow the following steps:

Step 1: Extract each item from the signature, by splitting on `,` separator.

```js
items = 't=1655795539264,v1=b6bc2aa82491c32f1cbef0eb52b7ffaa51467ea65a03b5d4ccdcfb9e0941c946'.split(',')
// [ 't=1655795539264', 'v1=b6bc2aa82491c32f1cbef0eb52b7ffaa51467ea65a03b5d4ccdcfb9e0941c946' ]
```

Step 2: Extract timestamp from the signature.

```js
timestamp = 't=1655795539264'.split('=')[1]
// '1655795539264'
```

Step 3: Extract HMAC hash from the signature.

```js
hmac = 'v1=b6bc2aa82491c32f1cbef0eb52b7ffaa51467ea65a03b5d4ccdcfb9e0941c946'.split('=')[1]
// 'b6bc2aa82491c32f1cbef0eb52b7ffaa51467ea65a03b5d4ccdcfb9e0941c946'
```

Step 4: Compute the HMAC hash using the webhook secret, signature timestamp and raw request body.

- HAMC key is the webhook secret.
- HMAC payload is formed by concatenating the timestamp(as numeric string), character `.` and raw request body (encoded as UTF8 string).
- Use SHA256 algorithm to compute the HMAC hash.
- HMAC hash is hex encoded.

```js
computeHmac = Hmac('sha256')
  .Key(webhookSecret)
  .Payload(timestamp + '.' + rawRequestBody)
  .Encode('hex')
```

Step 5: Compare the computed HMAC hash with the HMAC hash from signature.

```js
computeHmac === hmac
```

If the computed HMAC hash matches the HMAC hash in the signature, the webhook request can be considered valid.

## Preventing replay attacks

When an attacker intercepts a webhook request, he can replay the request multiple times with a valid signature.

To mitigate this, Imagekit webhook signature contains a timestamp. The timestamp is generated right before the webhook request is sent to the server.

For retriving this timestamp, the `verify` method in ImagekitSDK returns timestamp & parsed event object.

If timestamp is within the tolerance limit, the request can be considered as valid, else the request must be stalled.

> Optionally, a stronger approach is to use a nonce to prevent replay attacks. You can use Event ID as nonce, it is guaranteed to be unique across all events.
>
> You can find the event ID in `id` field of the event object.

## Sample Codes

{% tabs %}
{% tab title="ExpressJS" %}

```js
const express = require("express");
const { Webhook } = require("imagekit");

// Webhook configs
const WEBHOOK_ENDPOINT = "/webhook";
const WEBHOOK_SECRET = "whsec_EvCLlGyctj2WyIMW2r62m3JnyN4SXMsY"; // Copy from Imagekit dashboard
const WEBHOOK_EXPIRY_DURATION = 60 * 1000; // 60 seconds

// Server configs
const PORT = 8081;

const app = express();

app.post("/webhook", express.raw({ type: "*/*" }), (req, res) => {
  const signature = req.headers["x-ik-signature"];
  const rawBody = req.body;

  // Verify webhook signature
  let webhookResult;
  try {
    webhookResult = Webhook.verify(rawBody, signature, WEBHOOK_SECRET);
  } catch (e) {
    // Failed to verify webhook
    return res.status(401).send(`Webhook error: ${e.message}`);
  }
  const { timestamp, event } = webhookResult;

  // Check if webhook has expired
  if (timestamp.getTime() + WEBHOOK_EXPIRY_DURATION < Date.now()) {
    // Stall webhook
    return res.status(401).send("Webhook signature expired");
  }

  // Handle webhook
  switch (event.type) {
    case "video.transformation.accepted":
      console.log("Video transformation request accepted");
      break;
    case "video.transformation.ready":
      console.log("Video transformation ready");
      break;
    case "video.transformation.error":
      console.log("Video transformation error");
      break;
  }

  // Acknowledge webhook is received and processed successfully
  res.status(200).end();
});

app.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`);
  console.log(
    `Webhook endpoint: 'http://localhost:${PORT}${WEBHOOK_ENDPOINT}'`,
    "Do replace 'localhost' with public endpoint"
  );
});
```

{% endtab %}

{% tab title="Fastify" %}

```js
const fastify = require("fastify");
const fastifyRawBody = require("fastify-raw-body");
const { Webhook } = require("imagekit");

// Webhook configs
const WEBHOOK_ENDPOINT = "/webhook";
const WEBHOOK_SECRET = "whsec_EvCLlGyctj2WyIMW2r62m3JnyN4SXMsY"; // Copy from Imagekit dashboard
const WEBHOOK_EXPIRY_DURATION = 60 * 1000; // 60 seconds

// Server configs
const PORT = 8081;

const server = fastify();

const startServer = async (port) => {
  // Use 'fastify-raw-body' package to retrive raw request body as utf8 string
  await server.register(fastifyRawBody, { routes: [WEBHOOK_ENDPOINT] });

  // Add webhook endpoint route
  server.route({
    method: "POST",
    url: WEBHOOK_ENDPOINT,
    handler: (req, res) => {
      // Handle webhook
      const signature = req.raw.headers["x-ik-signature"];
      const rawBody = req.rawBody;

      // Verify webhook signature
      let webhookResult;
      try {
        webhookResult = Webhook.verify(rawBody, signature, WEBHOOK_SECRET);
      } catch (e) {
        // Webhook verification failed
        return res.status(401).send(`Webhook error: ${e.message}`);
      }
      const { timestamp, event } = webhookResult;

      // Check if webhook has expired
      if (timestamp.getTime() + WEBHOOK_EXPIRY_DURATION < Date.now()) {
        // Stall webhook
        return res.status(401).send("Webhook signature expired");
      }
      
      // Handle webhook
      switch (event.type) {
        case "video.transformation.accepted":
          console.log("Video transformation request accepted");
          break;
        case "video.transformation.ready":
          console.log("Video transformation ready");
          break;
        case "video.transformation.error":
          console.log("Video transformation error");
          break;
      }

      // Acknowledge webhook is received and processed successfully
      res.status(200).send();
    },
  });

  await server.listen({ port });
};

startServer(PORT).then(() => {
  console.log(`Server listening on port ${PORT}`);
  console.log(
    `Webhook endpoint: 'http://localhost:${PORT}${WEBHOOK_ENDPOINT}'`,
    "Do replace 'localhost' with public endpoint"
  );
});
```

{% endtab %}

{% tab title="Node HTTP" %}

```js
const http = require("http");
const { Webhook } = require("imagekit");

// Webhook configs
const WEBHOOK_ENDPOINT = "/webhook";
const WEBHOOK_SECRET = "whsec_EvCLlGyctj2WyIMW2r62m3JnyN4SXMsY"; // Copy from Imagekit dashboard
const WEBHOOK_EXPIRY_DURATION = 60 * 1000; // 60 seconds

// Server configs
const PORT = 8081;

const webhookRoute = (req, res) => {
  const signature = req.headers["x-ik-signature"];
  const reqChunks = [];
  req.setEncoding("utf8");
  req.on("data", (chunk) => {
    reqChunks.push(chunk);
  });
  req.on("end", () => {
    const rawBody = reqChunks.join("");

    // Verify webhook signature
    let webhookResult;
    try {
      webhookResult = Webhook.verify(rawBody, signature, WEBHOOK_SECRET);
    } catch (e) {
      // Webhook signature verification failed
      res.writeHead(401, `Webhook error: ${e.message}`);
      res.end();
      return;
    }
    const { timestamp, event } = webhookResult;

    // Check if webhook has expired
    if (timestamp.getTime() + WEBHOOK_EXPIRY_DURATION < Date.now()) {
      // Stall webhook
      res.writeHead(401, "Webhook signature expired");
      res.end();
      return;
    }

    // Handle webhook
    switch (event.type) {
      case "video.transformation.accepted":
        console.log("Video transformation request accepted");
        break;
      case "video.transformation.ready":
        console.log("Video transformation ready");
        break;
      case "video.transformation.error":
        console.log("Video transformation error");
        break;
    }

    // Acknowledge webhook is received and processed successfully
    res.writeHead(200, "OK");
    res.end();
  });
};

const server = http.createServer((req, res) => {
  switch (req.url) {
    case WEBHOOK_ENDPOINT:
      webhookRoute(req, res);
      break;
    default: {
      res.writeHead(404);
      res.end();
    }
  }
});

server.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`);
  console.log(
    `Webhook endpoint: 'http://localhost:${PORT}${WEBHOOK_ENDPOINT}'`,
    "Do replace 'localhost' with public endpoint"
  );
});

```

{% endtab %}

{% endtabs %}

## List of events

Below is the list of events for which ImageKit calls your configured webhook.

- [video.transformation.accepted](../../features/video-transformation/video-webhook-events.md#video.transformation.accepted)
- [video.transformation.ready](../../features/video-transformation/video-webhook-events.md#video.transformation.ready)
- [video.transformation.error](../../features/video-transformation/video-webhook-events.md#video.transformation.error)
