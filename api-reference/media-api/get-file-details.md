# Get file details

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/details" method="get" summary="Get file details API" %}
{% swagger-description %}
Get all the details and attributes of the current version of the file.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" required="false" %}
The unique fileId of the uploaded file. `fileId` is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="false" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will get file details in the JSON-encoded response body." %}
```javascript
{
    "fileId": "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt", "round-neck", "sale2019"],
    "AITags": [
        {
            "name": "Shirt",
            "confidence": 90.12,
            "source": "google-auto-tagging"
        },
        /* ... more googleVision tags ... */
    ],
    "versionInfo": {
            "id": "598821f949c0a938d57563bd",
            "name": "Version 1"
    },
    "isPrivateFile": false,
    "customCoordinates": null,
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "fileType": "image",
    "mime": "image/jpeg",
    "width": 100,
    "height": 100,
    "size": 100,
    "hasAlpha": false,
    "customMetadata": {
        brand: "Nike",
        color: "red"
    },
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z"
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the [file object](./#file-object-structure) in JSON-encoded response body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X GET "https://api.imagekit.io/v1/files/file_id/details" \
-u your_private_api_key:
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

imagekit.getFileDetails("file_id", function(error, result) {
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

details = imagekit.get_file_details(file_id="file_id")

print("File detail-", details, end="\n\n")

# Raw Response
print(details.response_metadata.raw)

# print that file's id
print(details.file_id)
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

$getFileDetails = $imageKit->getDetails("file_id");

echo("File details : " . json_encode($getFileDetails));
```
{% endtab %}

{% tab title="Java" %}
```java
Result result=ImageKit.getInstance().getFileDetail("file_id");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.get_file_deltails(file_id: 'file_id')
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.FileById(ctx, "file_id")
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
Result res1 = imagekit.GetFileDetail("file_Id");
```
{% endtab %}

{% endtabs %}
