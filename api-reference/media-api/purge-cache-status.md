# Purge cache status

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/purge/:requestId" method="get" summary="Purge cache status API" %}
{% swagger-description %}
Get the status of the submitted purge request.
{% endswagger-description %}

{% swagger-parameter in="path" name="requestId" type="string" %}
The 

`requestId`

 returned in the response of purge cache API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of 

`your_private_api_key:`

\




**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="200" description="The status can be either Pending or Completed" %}
```javascript
{
    status : "Pending" // or "Completed"
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the purge request status in a JSON-encoded response body.

### Understanding the response

The JSON-encoded response will have `status` property.

| Field  | Description                                                                                                                                                                                                                                                                                                                                                                 |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status | <p>The current status of a submitted purge request. It can be either:<br></p><ul><li><code>Pending</code> - The request has been successfully submitted, and purging is in progress.</li><li><code>Completed</code> - The purge request has been successfully completed. And now you should get a fresh object. Check the Age header in response to confirm this.</li></ul> |

## Examples

Here are some example requests to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# requestId returned in response of purge cache API.
curl -X GET "https://api.imagekit.io/v1/files/purge/requestId" \
-u your_private_key:
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

imagekit.getPurgeCacheStatus("cache_request_id", function(error, result) {
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

purge_cache_status = imagekit.get_purge_cache_status("cache_request_id")

print("Cache status-", purge_cache_status)
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

$purgeCacheStatus = $imageKit->purgeCacheApiStatus("cache_request_id");

echo("Purge cache status : " . json_encode($purgeCacheStatus));
```
{% endtab %}

{% tab title="Java" %}
```java
ResultCacheStatus result=ImageKit.getInstance().getPurgeCacheStatus("cache_request_id");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
purge_cache_status = imagekitio.purge_file_cache_status("cache_request_id")
```
{% endtab %}
{% endtabs %}
