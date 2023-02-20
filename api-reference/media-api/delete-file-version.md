# Delete file version

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/versions/:versionId" method="delete" summary="Delete file version API" %}
{% swagger-description %}
Delete any non-current version of a file.
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

{% swagger-response status="204" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="If the requested file version to delete is the current version of file" %}
```javascript
{
     "message" : "You cannot delete current version of a file.",
     "help" : "For support kindly contact us at support@imagekit.io ."
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If the requested file or file version is not found in the media library, then a 404 response is returned." %}
```javascript
{
     "message" : "The requested asset does not exist.",
     "help" : "For support kindly contact us at support@imagekit.io ."
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the file details in JSON-encoded response body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}

{% tab title="cURL" %}
```bash
# The unique fileId and versionId of the uploaded file. fileId and versionId (versionInfo.id) is returned in response of list files API and upload API.
curl -X DELETE "https://api.imagekit.io/v1/files/file_id/versions/version_id" \
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

var fileId = "file_id";
var versionId = "version_id"

imagekit.deleteFileVersion({
     fileId,
     versionId
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

delete_file_version = imagekit.delete_file_version(file_id="file_id", version_id="version_id")

print("Delete file version-", delete_file_version, end="\n\n")

# Raw Response
print(delete_file_version.response_metadata.raw)
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
$versionId = 'version_id';

$deleteFileVersion = $imageKit->deleteFileVersion($fileId, $versionId);
```
{% endtab %}

{% tab title="Java" %}
```java

DeleteFileVersionRequest deleteFileVersionRequest = new DeleteFileVersionRequest();
deleteFileVersionRequest.setFileId("file_id");
deleteFileVersionRequest.setVersionId("version_id");
ResultNoContent resultNoContent = ImageKit.getInstance().deleteFileVersion(deleteFileVersionRequest);
```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio.delete_file_version(
    file_id: 'file_id',
    version_id: 'version_id'
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.DeleteFileVersion(ctx, "file_id", "version_id")
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
DeleteFileVersionRequest delRequest = new DeleteFileVersionRequest
    {
        fileId = "file_Id",
        versionId = "version_Id"
    };
ResultNoContent resultNoContent1 = imagekit.DeleteFileVersion(delRequest);
```
{% endtab %}

{% endtabs %}
