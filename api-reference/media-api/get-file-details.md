# Get file details

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/details" method="get" summary="Get file details API" %}
{% swagger-description %}
Get all the file details and attributes.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" %}
The unique fileId of the uploaded file. 

`fileId`

 is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of 

`your_private_api_key:`

\




**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will get file details in the JSON-encoded response body." %}
```javascript
{
    "fileId" : "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt","round-neck","sale2019"],
    "isPrivateFile" : false,
    "customCoordinates" : null,
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "fileType": "image",
    "mime": "image/jpeg",
    "width": 100,
    "height": 100,
    "size": 100,
    "hasAlpha": false,
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z"
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the file details in JSON-encoded response body.

### Understanding response

The JSON-encoded response details of the file can have the following properties:

| Property name     | Description                                                                                                                                                                          |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| fileId            | The unique fileId of the uploaded file.                                                                                                                                              |
| type              | <p>Type of item. It can be either <code>file</code> or <code>folder</code>.<br></p>                                                                                                  |
| name              | Name of the file or folder.                                                                                                                                                          |
| filePath          | <p>The relative path of the file. In the case of an image, you can use this <br>path to construct different <a href="../../features/image-transformations/">transformations</a>.</p> |
| tags              | Array of tags associated with the image. If no tags are set, it will be `null`.                                                                                                      |
| isPrivateFile     | Is the file marked as private. It can be either `true` or `false`.                                                                                                                   |
| customCoordinates | <p>Value of custom coordinates associated with the image in format <code>x,y,width,height</code>. <br>If customCoordinates are not defined then it is <code>null</code>.</p>         |
| url               | A publicly accessible URL of the file.                                                                                                                                               |
| thumbnail         | In case of an image, a small thumbnail URL.                                                                                                                                          |
| fileType          | The type of file, could be either `image` or `non-image`.                                                                                                                            |
| mime              | MIME Type of the file. For example - `image/jpeg`                                                                                                                                    |
| height            | Height of the image in pixels (Only for images)                                                                                                                                      |
| width             | Width of the image in pixels (Only for Images)                                                                                                                                       |
| size              | Size of the image file in Bytes                                                                                                                                                      |
| hasAlpha          | TODO                                                                                                                                                                                 |
| createdAt         | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                         |
| updatedAt         | The date and time when the file was last updated. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                           |

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X GET "https://api.imagekit.io/v1/files/fileId/details" \
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

imagekit.getFileDetails("fileId", function(error, result) {
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

details = imagekit.get_file_details(file_id)

print("File detail-", details, end="\n\n")
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

$getFileDetails = $imageKit->getDetails($fileId);

echo("File details : " . json_encode($getFileDetails));
```
{% endtab %}

{% tab title="Java" %}
```java
Result result=ImageKit.getInstance().getFileDetail("fileId");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
details = imagekitio.get_file_details("fileId")
```
{% endtab %}
{% endtabs %}
