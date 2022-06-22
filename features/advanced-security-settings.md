# Advanced security settings

{% hint style="info" %}
**Paid plan only**\
This feature is only available in custom enterprise pricing plans.
{% endhint %}

ImageKit leverages AWS Cloudfront functions and Cloudfront distribution configurations to provides a set of advanced security and optimisation features that you can use to optimise your asset delivery flow and save costs.

The following features are included:
1. IP and IP range blocking.
2. Geographic restriction.
3. HTTP Referer restriction.
4. Improved caching and WebP image delivery for some mobile apps.

![Advanced security settings in ImageKit's dashboard](<../.gitbook/assets/advanced-security-settings.png>)

## How to configure?

If this feature is included in you plan, please reach out to us at support@imagekit.io to get it set up for your account. Once it is enabled and configured you can use this feature from your ImageKit account's dashboard.

## IP and IP range blocking

{% hint style="info" %}
**Limits**\
- You can block at most 100 absolute IPs or IP ranges.
- Only IPv4 blocking is supported.
{% endhint %}

This feature blocks absolute IPs or IP ranges from accessing content. Any request from these unauthorized IPs would result in an HTTP `403` error response and the asset will not be delivered.

To block IPs, you need to provide a list of valid absolute IPv4 addresses or IPv4 ranges in CIDR notation separated by new line in the input - using the advanced security settings in dashboard.

Examples of valid input- 
- Absolute IPv4- `163.120.4.15`
- IPv4 range using CIDR notation- `10.0.0.0/24`. This blocks all IPs in range `10.0.0.0` to `10.0.0.255`.

## Geographic restriction

ImageKit provides geographic restriction using [AWS Cloudfront distribution's Geo restriction feature](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html).

You can restrict access to assets from your distribution using geographic restriction at country level. Cloudfront uses `MaxMind` GeoIP databases to determine the location of the users. It also uses `ISO 3166-1-alpha-2` country codes.

There are 3 options for geographic restriction-

- Blacklist. 
- Whitelist.
- No restriction.

By default no geographic restrictions are applied.

## HTTP Referer restriction

{% hint style="info" %}
**Limits**\
You can whitelist or blacklist at most 30 referers.
{% endhint %}

Referer restriction ensures that content is only loaded from authorized domains. Any unauthorized request will result in an HTTP `403` error and the asset will not be delivered.

ImageKit provides 3 options for referer restriction-

- Blacklist. 
- Whitelist.
- No restriction.

For blacklisting or whitelisting domains, you need to provide a list of valid URLs (each URL must include at least the scheme and domain name) separated by new line. The exact domain mentioned in the URLs would be whitelisted or blocklisted.

For example- 
If you want to whitelist `ik.imagekit.io` you can input `https://ik.imagekit.io`.
If you input `https://developer.mozilla.org/en-US/search?q=URL`, the domain `developer.mozilla.org` will be set for restriction.

**Note:** Wildcard domain restrictions are not supported as of now.

### Uses
Blacklisting can be used to block domains that are hotlinking your content/assets which increases your bandwidth usage and hosting costs. This can result in significant cost savings if your assets are undesirably hotlinked on other websites.

Whitelisting can be used to allow only selected domains to access your assets. You can use it to serve content only on your whitelisted domains. Other referers would be blocked from loading your assets.

If you do not want any HTTP referer restriction you can opt for no restriction, which is the default behaviour.

## Improved caching and WebP image delivery for some mobile apps

Imagekit provides cache key normalisation for image delivery, and WebP image delivery for some mobile apps by leveraging AWS Cloudfront functions once the security settings are configured and used.