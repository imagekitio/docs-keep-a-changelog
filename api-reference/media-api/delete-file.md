# Delete file

You can programmatically delete uploaded files in media library using delete file API.

When you delete a file, all its transformations are also deleted. However, if a file or specific transformation has been requested in the past, then the response is cached in CDN. You can purge the cache from the CDN using [purge API](purge-cache.md).

{% api-method method="delete" host="https://api.imagekit.io" path="/v1/files/:fileId" %}
{% api-method-summary %}
Delete file API
{% endapi-method-summary %}

{% api-method-description %}
Deletes a file and all its transformations.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="fileId" type="string" required=true %}
The unique fileId of the uploaded file. `fileId` is returned in list files API and upload API 
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

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `204` status code with empty body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X DELETE "https://api.imagekit.io/v1/files/fileId" \
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

imagekit.deleteFile("file_id", function(error, result) {
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

delete = imagekit.delete_file("file_id")

print("Delete File-", delete)
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

$deleteFile = $imageKit->deleteFile("file_id");

echo("Delete file : " . json_encode($deleteFile));
```
{% endtab %}

{% tab title="Java" %}
```java
Result result=ImageKit.getInstance().deleteFile("file_id");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
delete = imagekitio.delete_file("file_id")
```
{% endtab %}
{% endtabs %}

