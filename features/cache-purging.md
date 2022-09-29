# Cache purging

ImageKit.io provides two methods to clear the cache for a particular image URL.

1. Using the Dashboard.
2. Using [API](../api-reference/media-api/purge-cache.md).

### Through the Dashboard

Navigate to [Purge Cache](https://imagekit.io/dashboard/purge-cache) within the dashboard

Click on 'New Request' at the top right corner of the page to open a popup.

![Purge cache from ImageKit dashboard](<../.gitbook/assets/purge-cache.png>)

Enter the full image URL for which you want to clear the cache. If the URL contains [path transformation parameters](image-transformations/#transformations-as-a-path-parameter) or [query transformation parameters](image-transformations/#transformations-as-a-query-parameter), add those parameters as well to clear the cache. You can enter multiple URLs separated by a new line, and a maximum of 100 URLs can be submitted in a single request.

#### Purge Cache for Multiple Files

You can purge the cache for multiple files within a directory by appending a wildcard at the end of the URL, only if **ANY ONE** of the following conditions are met:

1. The path consists of at least two levels of nesting: \
   The path of the directory, excluding your imagekitId and URL pattern, contains at least two levels of nesting, starting from the root as shown below:\
   &#x20;:white\_check\_mark: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/LEVEL_1/LEVEL_2*`\
   &#x20;:x: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/LEVEL_1*`\
   \
   For example, the path `/images/upload*` is valid, but `/images*` is not.\

2. The path length is at least 15 characters:\
   The path of the directory, excluding your imagekitId and URL pattern, is at least 15 characters in length, as shown below:\
   &#x20;:white\_check\_mark: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/FIFTEEN_CHARACTERS*`\
   \
   However, if the first condition is met, i.e. the path consists of at least two levels of nesting, then it need not be 15 characters long.\

3. The path is a complete file path:\
   The path specified is a complete path pointing to a file, with the file extension present at the end of the path, as shown below:\
   &#x20;:white\_check\_mark: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/FILE.EXT*`\
   \
   For example, the path `/sample.jpg*` is valid, despite not being 15 characters long, or having two levels of nesting.

Note that the wildcard can only be appended at the end of a URL. Wildcards within the path are not supported. i.e. `/folder/*/sample.jpg` is an invalid path. Please reach out to us at [support@imagekit.io](mailto:customer-support@imagekit.io) if you need to purge all images on a specific pattern not supported through the dashboard.

{% hint style="danger" %}
The following paths are unconditionally not supported when using a wildcard. Please reach out to us at support@imagekit.io if you need to purge the following patterns.

* `media/catalog/*`
* `s/files/*`
{% endhint %}

Click 'Submit' and allow a few minutes for the cache to clear. It would be best if you refreshed the dashboard to get the updated cache clear status.

If the cache does not clear within 15 minutes, please create a support ticket from the [dashboard](https://imagekit.io/dashboard) or email us at [support@imagekit.io](mailto:customer-support@imagekit.io).

{% hint style="danger" %}
Clearing the cache through the dashboard does not clear the cache within the user's browser. It only removes the cache for the URL at the CDN and ImageKit.io's [internal caches](caches.md#internal-caching). Browser cache is cleared only when the object/file associated with the URL expires or is forcefully cleared by the end-user. We recommend implementing versioning of your static resource URLs to ensure instant updates for all your users without having to clear the cache.
{% endhint %}

## Limits on Cache Clear

You can clear 1000 URL caches in a month from a single account (from both the dashboard and [Purge API](../api-reference/media-api/purge-cache.md) combined). When you reach this limit, cache clear requests get blocked on your account. If you need more cache clear requests within a month, please create a support ticket through the dashboard or email us at [support@imagekit.io](mailto:customer-support@imagekit.io).
