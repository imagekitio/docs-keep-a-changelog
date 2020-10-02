# Purge cache status

{% api-method method="get" host="https://api.imagekit.io" path="/v1/files/purge/:requestId" %}
{% api-method-summary %}
Purge cache status API
{% endapi-method-summary %}

{% api-method-description %}
Get the status of the submitted purge request.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="requestId" type="string" required=true %}
The `requestId` returned in the response of purge cache API.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The status can be either `Pending` or `Completed`
{% endapi-method-response-example-description %}

```javascript
{
    status : "Pending" // or "Completed"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code \(application/JSON\)

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the purge request status in a JSON-encoded response body.

### Understanding the response

The JSON-encoded response will have `status` property.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">
        <p>The current status of a submitted purge request. It can be either:
          <br
          />
        </p>
        <ul>
          <li><code>Pending</code> - The request has been successfully submitted, and
            purging is in progress.</li>
          <li><code>Completed</code> - The purge request has been successfully completed.
            And now you should get a fresh object. Check the Age header in response
            to confirm this.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

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

