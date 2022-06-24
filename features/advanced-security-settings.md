# Advanced security settings

{% hint style="info" %}
**Enterprise plan only**\
This feature is only available in custom enterprise pricing plans.
{% endhint %}

ImageKit provides advanced security and optimization features that you can use to optimize your asset delivery flow and save costs.

It includes the following features:
1. IP and IP range blocking
2. Geographic restriction
3. HTTP Referrer-based restriction
4. Improved caching and WebP image delivery for some mobile apps

![Advanced security settings in ImageKit's dashboard](<../.gitbook/assets/advanced-security-settings.png>)

## How to configure?

If your plan includes this feature, please contact us at support@imagekit.io to set it up for your account. Once it is enabled and configured, you can use this feature from your ImageKit account's dashboard.

## IP and IP range blocking

{% hint style="info" %}
**Limits**\
- You can block at most 100 absolute IPs or IP ranges.
- Only IPv4 blocking is supported.
{% endhint %}

This feature blocks absolute IPs or IP ranges from accessing content. Any request from these unauthorized IPs would result in an HTTP `403` error response, and the asset will not be delivered.

To block IPs, you need to provide a list of valid absolute IPv4 addresses or IPv4 ranges in CIDR notation separated by a new line in the input - using the advanced security settings in the dashboard.

Examples of valid input- 
- Absolute IPv4- `163.120.4.15`
- IPv4 range using CIDR notation- `10.0.0.0/24`. This blocks all IPs in range `10.0.0.0` to `10.0.0.255`.

## Geographic restriction

ImageKit provides geographic restriction that allows you to restrict access to assets from your distribution at the country level.

There are three options for geographic restriction-

- Blocklist
- Allowlist
- No restriction

By default, no geographic restrictions are applied.

## HTTP Referrer-based restriction

{% hint style="info" %}
**Limits**\
You can allow or block at most 30 referrers.
{% endhint %}

The Referer HTTP request header contains an absolute or partial address of the page that makes the request. The Referer header allows a server to identify a page where people are visiting it from.

Referrer-based restriction ensures that content only loads on requests from authorized domains. Any unauthorized request will result in an HTTP `403` error, and the asset will not be delivered.

ImageKit provides three options for referrer restriction-

- Blocklist 
- Allowlist
- No restriction

For blocking or allowing domains, you need to provide a list of valid URLs (Each URL must include at least the scheme and domain name) separated by a new line. The exact domain mentioned in the URLs would be allowed or blocked.

For example:
- If you add `https://ik.imagekit.io` to the allowlist, the URL `ik.imagekit.io` will be allowed.
- If you add `https://developer.mozilla.org/en-US/search?q=URL` to the blocklist, the domain `developer.mozilla.org` will be restricted.

**Note:** Wildcard domain restrictions are not supported as of now.

### Uses
Using the blocklist will block domains that are hotlinking your content or assets, increasing your bandwidth usage and hosting costs. The blocklist saves costs significantly if your assets are undesirably hotlinked on other websites.

The allowlist can be used to allow only selected domains to access your assets. For example, you can use it to serve content only on your allowed domains and automatically block other referrers.

If you do not want any HTTP referrer restriction, you can opt for no restriction, which is the default behavior.

## Improved caching and WebP image delivery for some mobile apps

ImageKit provides cache key normalization for image delivery and WebP image delivery for some mobile devices once the security settings are configured and used.
