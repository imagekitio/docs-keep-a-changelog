# Non-Image File Compression

ImageKit.io supports the delivery of images and non-image files using its CDN. All non-image files delivered via ImageKit.io CDN are compressed using Brotli and gzip compression, with preference given to Brotli compression over gzip compression.

Over 90% of browsers currently support Brotli compression.

If the browser supports Brotli compression and is advertised within the Accept Encode Request Header, ImageKit.io uses Brotli compression. Otherwise, gzip compression is used by default. All the resources compressed using either algorithm is cached at the CDN, and all the end users get a cached copy of the resource.

{% hint style="info" %}
Images are not subject to Brotli or gzip compression. All image formats have their own compression algorithms suited for that particular format and this is all taken care of by ImageKit.io.
{% endhint %}

## Checking for Brotli Compression

Support for Brotli compression with a browser can be checked within its Accept Encode Request Header. The browsers that support Brotli would include 'br' as shown below.

![](../.gitbook/assets/brotli-compression.jpg)

Look at the Content Encoding Response Header to check whether the resource is Brotli encoded.

![Content-encoding set to br](../.gitbook/assets/brotli-compression-content-encoding.jpg)

![Content-encoding set to gzip](../.gitbook/assets/gzip-content-encoding.jpg)

If you have any queries regarding the compressions performed on non-image files, please reach out to our team through your dashboard or at [support@imagekit.io](mailto:supprort@imagekit.io).

