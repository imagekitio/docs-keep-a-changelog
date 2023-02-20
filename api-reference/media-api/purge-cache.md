# Purge cache

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/purge" method="post" summary="Purge cache API" %}
{% swagger-description %}
This will purge CDN and ImageKit.io's internal cache.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="url" type="string" %}
The exact URL of the file to be purged. For example - `https://ik.imageki.io/your_imagekit_id/rest-of-the-file-path.jpg`.
{% endswagger-parameter %}

{% swagger-response status="201" description="On success, you will receive a requestId which can be used to get the purge request status." %}
```javascript
{
    requestId : "598821f949c0a938d57563bd"
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `201` status code with the request ID returned in the JSON-encoded response body.

`requestId` can be used to fetch the status of the submitted purge request.

### Purge Cache for Multiple Files

You can purge the cache for multiple files within a directory by appending a wildcard at the end of the URL, only if **ANY ONE** of the following conditions are met:

1. The path consists of at least two levels of nesting: \
   The path of the directory, excluding your imagekitId and URL pattern, contains at least two levels of nesting, starting from the root as shown below:\
    :white_check_mark: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/LEVEL_1/LEVEL_2*`\
    :x: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/LEVEL_1*`\
   \
   For example, the path `/images/upload*` is valid, but `/images*` is not.\

2. The path length is at least 15 characters:\
   The path of the directory, excluding your imagekitId and URL pattern, is at least 15 characters in length, as shown below:\
    :white_check_mark: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/FIFTEEN_CHARACTERS*`\
   \
   However, if the first condition is met, i.e. the path consists of at least two levels of nesting, then it need not be 15 characters long.\

3. The path is a complete file path:\
   The path specified is a complete path pointing to a file, with the file extension present at the end of the path, as shown below:\
    :white_check_mark: `https://ik.imagekit.io/IMAGEKIT_ID/PATTERN/FILE.EXT*`\
   \
   For example, the path `/sample.jpg*` is valid, despite not being 15 characters long, or having two levels of nesting.

Note that the wildcard can only be appended at the end of a URL. Wildcards within the path are not supported. i.e. `/folder/*/sample.jpg` is an invalid path.

{% hint style="danger" %}
The following paths are unconditionally not supported when using a wildcard. Please reach out to us at support@imagekit.io if you need to purge the following patterns.

* `media/catalog/*`
* `s/files/*`
{% endhint %}

### Limitations

Purge API has the following limitations:

* An account can issue a maximum of 1000 purge requests in a month. Please reach out to us at [support@imagekit.io](mailto:support@imagekit.io) if you need to increase this limit.
* Note that the wildcard can only be appended at the end of a URL. Wildcards within the path are not supported. i.e. `/folder/*/sample.jpg` is an invalid path. Please reach out to us at [support@imagekit.io](mailto:support@imagekit.io) if you need to purge all images on a specific pattern not supported through the API.

## Examples

Here are some example requests to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/files/purge" \
-H "content-type: application/json" \
-u your_private_key: -d'
{
    "url": "https://ik.imagekit.io/your_imagekit_id/default-image.jpg"
}
'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

imagekit.purgeCache("https://ik.imagekit.io/your_imagekit_id/default-image.jpg", function(error, result) { 
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your_public_api_key',
    private_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

purge_cache = imagekit.purge_cache(file_url="https://ik.imagekit.io/your_imagekit_id/default-image.jpg")

print("Purge cache-", purge_cache)

# Raw Response
print(purge_cache.response_metadata.raw)

# print the purge file cache request id
print(purge_cache.request_id)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_api_key";
$your_private_key = "your_private_api_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$image_url = 'https://ik.imagekit.io/your_imagekit_id/default-image.jpg';
$purgeCache = $imageKit->purgeCache($image_url);

echo("File details : " . json_encode($purgeCache));
```
{% endtab %}

{% tab title="Java" %}
```java
ResultCache result=ImageKit.getInstance().purgeCache("https://ik.imagekit.io/your_imagekit_id/default-image.jpg");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
purge_cache = imagekitio.purge_file_cache(file_url: "https://ik.imagekit.io/your_imagekit_id/default-image.jpg")
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.PurgeCache(ctx, media.PurgeCacheParam{
    Url: "https://ik.imagekit.io/your_imagekit_id/default-image.jpg",
})
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
ResultCache resultCache = imagekit.PurgeCache("https://ik.imagekit.io/your_imagekit_id/default-image.jpg");
```
{% endtab %}

{% endtabs %}
