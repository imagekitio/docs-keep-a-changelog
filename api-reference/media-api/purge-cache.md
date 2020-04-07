# Purge cache

{% api-method method="post" host="https://api.imagekit.io" path="/v1/files/purge" %}
{% api-method-summary %}
Purge cache API
{% endapi-method-summary %}

{% api-method-description %}
This will purge CDN and ImageKit.io internal cache.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`   
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="url" type="string" required=true %}
The exact URL of the file to be purged. For example - `https://ik.imageki.io/your_imagekit_id/rest-of-the-file-path.jpg`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
On success, you will receive a `requestId` which can be used to get the purge request status.
{% endapi-method-response-example-description %}

```javascript
{
    requestId : "598821f949c0a938d57563bd"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the request ID returned in JSON-encoded response body.

`requestId` can be used to fetch the status of submitted purge request.

### Limitations

Purge API has following limitations:

* An account can issue a maximum of 1000 purge requests in a month. Please reach out to us at [support@imagekit.io](mailto:support@imagekit.io) if you need to increase this limit.
* Purge API doesn't accept wildcard in `url`. Please reach out to us at [support@imagekit.io](mailto:support@imagekit.io) if you need to purge all images on a specific pattern.

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
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

purge_cache = imagekit.purge_cache(file_url="https://ik.imagekit.io/your_imagekit_id/default-image.jpg")

print("Purge cache-", purge_cache)
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

$purgeCache = $imageKit->purgeCacheApi(array(
    "url" => "https://ik.imagekit.io/your_imagekit_id/default-image.jpg"
));

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
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
purge_cache = imagekitio.purge_file_cache("https://ik.imagekit.io/your_imagekit_id/default-image.jpg")
```
{% endtab %}
{% endtabs %}

