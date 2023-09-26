# Get all versions of a file

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/versions" method="get" summary="Get all versions of an asset" %}
{% swagger-description %}
Get all versions of an asset using this API.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" required="true" %}
The unique fileId of the uploaded file. `fileId` is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="200" description="An array of file objects is returned." %}
```javascript
[
    {
        "fileId": "598821f949c0a938d57563bd",
        "type": "file-version",
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
        "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg?ik-obj-version=bREnN9Z5VQQ5OOZCSvaXcO9SW.su4QLu",
        "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg?ik-obj-version=bREnN9Z5VQQ5OOZCSvaXcO9SW.su4QLu",
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
    },
    ...more items
]
```
{% endswagger-response %}

{% swagger-response status="404" description="If the requested asset is not found in the media library, then a 404 response is returned." %}
```javascript
{
     "message" : "The requested asset does not exist.",
     "help" : "For support kindly contact us at support@imagekit.io ."
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the list of [file object](./#file-object-structure) in the JSON-encoded response body on success.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X GET "https://api.imagekit.io/v1/files/file_id/versions" \
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

imagekit.getFileVersions("file_id", function(error, result) {
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

file_versions = imagekit.get_file_versions(file_id='file_id')

print("Get File versions-", "\n", file_versions)

# Raw Response
print(file_versions.response_metadata.raw)

# print that file's version id
print(file_versions.list[0].version_info.id)
```
{% endtab %}

{% tab title="PHP" %}
```php
$public_key = "your_public_api_key";
$your_private_key = "your_private_api_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$fileId = 'file_id';

$getFileVersions = $imageKit->getFileVersions($fileId);

echo("File Versions : " . json_encode($getFileVersions));
```
{% endtab %}

{% tab title="Java" %}
```java

String fileId = "file_id";
ResultFileVersions resultFileVersions = ImageKit.getInstance().getFileVersions(fileId);

```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.file_versions(
    file_id: 'file_id'
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.FileVersions(ctx, media.FileVersionsParam{
    FileId: "file_id",
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
ResultFileVersions resultFileVersions = imagekit.GetFileVersions("file_Id");
```
{% endtab %}

{% endtabs %}
