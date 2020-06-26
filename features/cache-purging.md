# Cache purging

ImageKit.io provides two methods to clear the cache for a particular image URL.

1. Using the Dashboard.
2. Using [API](../api-reference/media-api/purge-cache.md).

### Through the Dashboard

Navigate to [Purge Cache](https://imagekit.io/dashboard#cache) within the dashboard

Click on 'New Request' at the top right corner of the page to open a popup.

![](../.gitbook/assets/screenshot-2020-06-15-at-1.11.33-pm.png)

Enter the full image URL for which you want to clear the cache. If the URL contains [path transformation parameters](image-transformations/#transformations-as-a-path-parameter) or [query transformation parameters](image-transformations/#transformations-as-a-query-parameter), add those parameters as well to clear the cache. You can enter multiple URLs separated by a new line, and a maximum of 100 URLs can be submitted in a single request.

You can purge the cache for all variants of an image like this:

> https://ik.imagekit.io/demo/default-image.jpg\*

Note that the wildcard can only be appended at the end of a complete image path \(must include extension\). Wildcards within the path are not supported by the dashboard.

{% hint style="info" %}
Please reach out to the ImageKit.io team at [support@imagekit.io](mailto:customer-support@imagekit.io) to clear the cache for the entire account or for a pattern.
{% endhint %}

Click 'Submit' and allow a few minutes for the cache to clear. It would be best if you refreshed the dashboard to get the updated cache clear status.

If the cache does not clear within a reasonable time, please create a support ticket from the [dashboard](https://imagekit.io/dashboard) or email us at [support@imagekit.io](mailto:customer-support@imagekit.io).

{% hint style="danger" %}
Clearing the cache through the dashboard does not clear the cache within the user's browser. It only removes the cache for the URL at the CDN and ImageKit.io's [internal caches](caches.md#internal-caching). Browser cache is cleared only when the object/file associated with the URL expires or is forcefully cleared by the end-user. We recommend implementing versioning of your static resource URLs to ensure instant update for all your users without having to clear the cache.
{% endhint %}

## Limits on Cache Clear

You can clear 1000 URL caches in a month from a single account \(from both the dashboard and [Purge API](../api-reference/media-api/purge-cache.md) combined\). When you reach this limit, cache clear requests get blocked on your account. If you need more cache clear requests within a month, please create a support ticket through the dashboard or email us at [support@imagekit.io](mailto:customer-support@imagekit.io).

