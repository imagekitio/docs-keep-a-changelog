# Rate Limits

Except for upload API, all our APIs are rate-limited to protect our infrastructure from excessive request rates and to keep ImageKit.io fast and stable for everyone.

Rate limiting is done across the account. When you exceed the rate limits for an endpoint, you will receive a `429` (Too many requests) response code.

{% hint style="info" %}
**Staying within the rate limits**

If you receive a response status code of 429 (Too Many Requests), please sleep/pause for the number of milliseconds specified by the value of `X-RateLimit-Reset` response header before making additional requests to that endpoint.
{% endhint %}

## Interval window

Rate limits are specified for a specific interval. So if an endpoint allows 100 requests per second (1000 milliseconds), and you exceed this rate, your application will receive a 429 response code. Every endpoint can have different limits, and these limits are exposed via rate-limiting headers, as explained below.

A single endpoint can have multiple limits, for example, a 100 request per second limit along with 1000 limits for a 15-minute duration.

## Response headers to understand rate limits

In response to every API request, you will also receive the following response headers. Use these headers to stay within rate limits or pause/sleep your workflow in case you exceed the limits.

| Response header                              | Description                                                                                                                                                     |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>X-RateLimit-Limit</code><br></p>    | <p>The maximum number of requests that can be made to this endpoint in the interval specified <br>by <code>X-RateLimit-Interval</code> response header.<br></p> |
| `X-RateLimit-Reset`                          | The amount of time in milliseconds, before you can make another request to this endpoint. Pause/sleep your workflow for this duration.                          |
| <p><code>X-RateLimit-Interval</code><br></p> | The duration of interval in milliseconds for which this rate limit was exceeded.                                                                                |

## Request higher rate limits

If your workflow demands a higher rate limit, please reach out to us at [support@imagekit.io](mailto:support@imagekit.io).
