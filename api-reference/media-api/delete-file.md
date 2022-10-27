# Delete file

You can programmatically delete uploaded files in the media library using delete file API.

{% hint style="info" %}
If a file or specific transformation has been requested in the past, then the response is cached. Deleting a file does not purge the cache. You can purge the cache using [purge API](purge-cache.md).
{% endhint %}

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId" method="delete" summary="Delete file API" %}
{% swagger-description %}
Deletes a file and all its versions from the media library.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" %}
The unique fileId of the uploaded file. `fileId` is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="204" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `204` status code with an empty body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X DELETE "https://api.imagekit.io/v1/files/file_id" \
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
    public_key='your_public_api_key',
    private_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

delete = imagekit.delete_file(file_id="file_id")

print("Delete File-", delete)

# Raw Response
print(delete.response_metadata.raw)
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

$fileId = 'file_id';

$deleteFile = $imageKit->deleteFile($fileId);

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
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
delete = imagekitio.delete_file(file_id: "file_id")
```
{% endtab %}

{% tab title="Go" %}
```go
ik, err := imagekit.New()

resp, err := ik.Media.DeleteFile(ctx, "file_id")

```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
ResultDelete res2 = imagekit.DeleteFile("file_Id");
```
{% endtab %}

{% endtabs %}
