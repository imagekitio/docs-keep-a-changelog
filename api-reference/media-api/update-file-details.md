# Update file details

{% api-method method="patch" host="https://api.imagekit.io" path="/v1/files/:fileId/details" %}
{% api-method-summary %}
Update file details API
{% endapi-method-summary %}

{% api-method-description %}
Update file details such as `tags` and `customCoordinates` attribute using update file detail API.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="fileId" type="string" required=true %}
The unique fileId of the uploaded file. `fileId` is returned in list files API and upload API.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="tags" type="array" required=false %}
An array of tags associated with the file e.g. `["tag1", "tag2"]`. If you want to unset it send `null`.
{% endapi-method-parameter %}

{% api-method-parameter name="customCoordinates" type="string" required=false %}
Define an important area in the image in the format `x,y,width,height` e.g. `10,10,100,100`. If you want to unset this send `null`.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
On success, you will receive the updated file details in the JSON-encoded response body.
{% endapi-method-response-example-description %}

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
    "createdAt": "2019-08-24T06:14:41.313Z"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the updated file details in JSON-encoded response body.

### Understanding response

| Proper name | Description |
| :--- | :--- |
| fileId | The unique fileId of the uploaded file.  |
| type | Type of item. It can either be `file` or `folder`. |
| name | Name of the file or folder. |
| filePath | The relative path of the file. In the case of an image, you can use this  path to construct different transformations. |
| tags | Array of tags associated with the image. If no tags are set, this will be `null`. |
| isPrivateFile | Is the file marked as private. It can be either `true` or `false`. |
| customCoordinates | Value of custom coordinates associated with the image in format `x,y,width,height`. If customCoordinates are not defined, it will be `null`. |
| url | A publicly accessible URL of the file. |
| thumbnail | In the case of an image, a small thumbnail URL. |
| fileType | The type of file could be either `image` or `non-image`. |
| mime | MIME Type of the file. For example - `image/jpeg` |
| width | Width of the image in pixels \(Only for Images\) |
| height | Height of the image in pixels \(Only for images\) |
| size | Size of the image file in bytes |
| hasAlpha | Whether the image has an alpha component or not. Can be `true` or `false` |
| createdAt | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ` |

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X PATCH "https://api.imagekit.io/v1/files/fileId/details" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "tags": [
        "image_tag"
    ],
    "customCoordinates": "10,10,100,100"
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

imagekit.updateFileDetails("file_id", { 
    tags : ['image_tag'],
    customCoordinates : "10,10,100,100"
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
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

updated_detail = imagekit.update_file_details(
    "file_id",
    {"tags": ["image_tag"], "custom_coordinates": "10,10,100,100"},
)

print("Updated detail-", updated_detail, end="\n\n")
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

$updateFileDetails = $imageKit->updateFileDetails("file_id", array("tags" => ['image_tag'], "customCoordinates" => "10,10,100,100"));

echo("Updated detail : " . json_encode($updateFileDetails));
```
{% endtab %}

{% tab title="Java" %}
```java
 FileUpdateRequest fileUpdateRequest =new FileUpdateRequest("file_id");
 List<String> tags=new ArrayList<>();
 tags.add("image_tag");
 fileUpdateRequest.setTags(tags);
 fileUpdateRequest.setCustomCoordinates("10,10,100,100");
 Result result=ImageKit.getInstance().updateFileDetail(fileUpdateRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
updated_detail = imagekitio.update_file_details(
    "file_id",
    {
        "tags": ['image_tag'],
        "custom_coordinates": "10,10,100,100"
    }
)
```
{% endtab %}
{% endtabs %}

