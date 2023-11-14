# Caches

ImageKit.io provides two ways to deliver images for your website - from the [media library](../media-library/overview/), or your [server/storage](../integration/configure-origin/). Each of these image sources follow certain caching rules pre-determined by ImageKit.io. Additionally, you can define your [own caching rules](caches.md#user-defined-caching), if needed.

## Internal Caching

ImageKit.io caches a copy of every transformed and optimized image at the CDN. Additionally, ImageKit.io also maintains its internal caches, which are co-located with its processing engine across [6 global locations](../media-library/overview/#where-is-the-imagekit-io-media-library-available-geographically). In case any URL is missed by the CDN, internal caches deliver the resources without passing on the request to your server or storage. The same process is followed when you integrate ImageKit.io with a [custom CDN](../testing-and-infrastructure-setup/integrate-with-your-cdn.md).

## Default Caching Rules

1. The original copies of the images and files are stored permanently within the [Media library](../media-library/overview/) when using the Media Library to store and deliver the images.
2. When using your server or storage as an [origin](../integration/configure-origin/) to deliver images, the original copies of the images and files stay within your servers, and ImageKit.io does not control how long the files remain there. Imagekit.io will access the original file from your origin and performs all the optimizations on this file. If you have the setting for "Save a copy of original images" switched on for your Enterprise account, ImageKit maintains a copy of your original image as well within its [internal caches](caches.md#internal-caching) and performs all the optimizations on this copy of the original instead of going to your origin to access the image.
3. The transformed and optimized copies of the images and files, when requested via a URL, are stored in line with the cache headers. This not only improves the performance for repeat requests, but ensures the number of requests made to your origin server or storage is less. However, ImageKit.io might change this practice for a specific account or all accounts, if and when necessary.
4. Every image URL is cached at the CDN, and other ImageKit.io [internal caches](caches.md#internal-caching), for a maximum of 180 days. This is done to ensure fast delivery of images across the globe.

{% hint style="info" %}
Files that are explicitly uploaded to the media library are charged as per your pricing plan. You are not billed for the storage consumed by the transformed and optimized copies of the files. The "Save a copy of original images" feature is separately chargeable on Enterprise plans. You can discuss this with your Customer Success Manager or Account Executive.
{% endhint %}

## Origin-Based Cache Control

`Cache-Control` header is a general header that specifies the caching policies of server responses. It provides instructions to both the browser and intermediate caches about how a particular resource should be cached. <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control" target="_blank">Learn more</a>

Origin-Based Cache Control feature allows caching based on the cache control headers being passed from your [origin](../integration/configure-origin/) attached to ImageKit.io. For example, if your origin (server or storage) sends a cache-control header to cache a file for 1 hour, ImageKit.io applies the cache-control header across all its internal caches, generated transformations, and CDN. This ensures that the cache control set by you is obeyed at all times.

Using the ImageKit dashboard, you may enable 'Origin-Based Cache Control' for any URL endpoint in your ImageKit account.

To enable this feature on an endpoint, follow these steps:

1. Go to the Advanced Settings tab of the URL endpoint.
2. Turn on 'Enable origin cache control' toggle.
3. Click save.

![Enable origin cache control demo](<../.gitbook/assets/screen-shot-enable-orig-cache-control.png>)

## FAQ

### How to define cache control headers for Web server origin?

For a 'Web Folder' or 'Web Proxy' origin, the `cache-control` header in response will be obeyed, if 'Origin-Based Cache Control' is enabled for the URL endpoint.

### How to define cache control headers for S3 or S3 compatible storage?

For S3 or S3 compatible storage, the `cache-control` header of the S3 object will be obeyed, if 'Origin-Based Cache Control' is enabled for the URL endpoint.

### Can I use `Age` header along with 'cache-control' header?

As of now, `Age` header from origin is not support.

### Can I use `Expires` header?

As of now, `Expires` header from origin is not support.

### Can I use Cache control feature in videoAPI?

Origin based cache control feature is not support by Imagekit's VideoAPI.

### Can I define caching rules for MediaLibrary assets?

Cache-control can be defined for the files that are being delivered from your storage or server attached to ImageKit.io, but not for the files stored within the [Media library](../media-library/overview/). However, you can use the cache purging feature to remove any file from the cache and force ImageKit.io to fetch the latest version of the file from the Media Library
