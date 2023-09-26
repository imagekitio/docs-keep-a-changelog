# Restore file version

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/versions/:versionId/restore" method="put" summary="Restore file version API" %}
{% swagger-description %}
Restore file version to a different version of a file.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" required="true" %}
The unique fileId of the uploaded file. `fileId` is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="versionId" type="string" required="true" %}
The unique versionId of the uploaded file's version. This is returned in list files API and upload API as `id` within the `versionInfo` parameter.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will get file details of the restored file in the JSON-encoded response body." %}
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
            "id": "697821f849c0a938d57563ce",
            "name": "Version 2"
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
    "updatedAt": "2019-09-24T06:14:41.313Z"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If the requested file is not found in the media library, then a 404 response is returned." %}
```javascript
{
     "message": "The requested file does not exist.",
     "help": "For support kindly contact us at support@imagekit.io ."
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If the requested file version does not exist for that file, then a 404 response is returned." %}
```javascript
{
     "message": "The requested file version does not exist.",
     "help": "For support kindly contact us at support@imagekit.io ."
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
# The unique fileId and versionId of the uploaded file. fileId and versionId (versionInfo.id) is returned in response of list files API and upload API.
curl -X PUT "https://api.imagekit.io/v1/files/file_id/versions/version_id/restore" \
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

imagekit.restoreFileVersion({
     fileId: "file_id",
     versionId: "version_id"
}, function(error, result) {
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

restore_file_version = imagekit.restore_file_version(file_id="file_id", version_id="version_id")

print("Restore file version-", restore_file_version, end="\n\n")

# Raw Response
print(restore_file_version.response_metadata.raw)

# print that file's id
print(restore_file_version.file_id)
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
$versionId = 'version_id';

$restoreFileVersion = $imageKit->restoreFileVersion([
    'fileId' => $fileId,
    'versionId' => $versionId,
]);

echo("Restore File  Version : " . json_encode($restoreFileVersion));
```
{% endtab %}

{% tab title="java" %}
```java
Result result = ImageKit.getInstance().restoreFileVersion("file_id", "version_id");
```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio.restore_file_version(
    file_id: 'file_id',
    version_id: 'version_id'
)
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
Result result = imagekit.RestoreFileVersion("file_Id", "version_Id");
```
{% endtab %}

{% endtabs %}
