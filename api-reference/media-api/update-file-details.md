# Update file details

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/details" method="patch" summary="Update file details API" %}
{% swagger-description %}
Update file details such as 

`tags`

, 

`customCoordinates`

attributes, remove existing 

`AITags`

and apply 

[extensions](../../extensions/overview/)

 using update file detail API.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" required="false" %}
The unique fileId of the uploaded file.

`fileId` is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="false" %}
base64 encoding of

`your_private_api_key:`



**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="removeAITags" type="array" required="false" %}
An array of AITags associated with the file that you want to remove e.g. `["car", "vehicle", "motorsports"]`. If you want to remove all

`AITags`associated with the file, send a string -

`"all"`.



**Note:** Remove operation for`AITags`executes before any of the `extensions`are processed**.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="webhookUrl" type="string" required="false" %}
Final status of pending extensions will be sent to this URL. To learn more about how ImageKit uses webhooks, refer here.

\\

https://docs.imagekit.io/extensions/overview#webhooks
{% endswagger-parameter %}

{% swagger-parameter in="body" name="extensions" type="array" required="false" %}
Stringified JSON object with array of extensions to be processed on the image.

\\

\\

For reference about extensions read here.

\\

https://docs.imagekit.io/extensions/overview
{% endswagger-parameter %}

{% swagger-parameter in="body" name="tags" type="array" required="false" %}
An array of tags associated with the file e.g.

`["tag1", "tag2"]`

. If you want to unset it send

`null`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="customCoordinates" type="string" required="false" %}
Define an important area in the image in the format

`x,y,width,height`

e.g.

`10,10,100,100`

. If you want to unset this send

`null`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive the updated file details in the JSON-encoded response body." %}
```javascript
/*
    Note: this example response is when extensions are not applied
    in update file details API request
*/
{
    "fileId" : "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt","round-neck","sale2019"],
    "AITags" null,
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

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the updated file details in JSON-encoded response body.

### Understanding response

| Property name     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fileId            | The unique `fileId` of the uploaded file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| type              | <p>Type of item. It can be either <code>file</code> or <code>folder</code>.<br></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| name              | Name of the file or folder.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| filePath          | <p>The relative path of the file. In the case of an image, you can use this <br>path to construct different <a href="../../features/image-transformations/">transformations</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                               |
| tags              | Array of tags associated with the image. If no tags are set, it will be `null`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| AITags            | Array of `AITags` associated with the image. If no `AITags` are set, it will be `null`. These tags can be added using the `google-auto-tagging` or `aws-auto-tagging` [extensions](../../extensions/overview/ai-based-auto-tagging.md).                                                                                                                                                                                                                                                                                                                                            |
| isPrivateFile     | Is the file marked as private. It can be either `true` or `false`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| customCoordinates | <p>Value of custom coordinates associated with the image in format <code>x,y,width,height</code>.<br>If customCoordinates are not defined then it is <code>null</code>.</p>                                                                                                                                                                                                                                                                                                                                                                                                        |
| url               | A publicly accessible URL of the file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| thumbnail         | In case of an image, a small thumbnail URL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| fileType          | The type of file, could be either `image` or `non-image`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| mime              | MIME Type of the file. For example - `image/jpeg`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| height            | Height of the image in pixels (Only for images)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| width             | Width of the image in pixels (Only for Images)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| size              | Size of the image file in Bytes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| hasAlpha          | TODO                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| createdAt         | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| updatedAt         | The date and time when the file was last updated. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| extensionStatus   | <p>Extension names with their processing status at the time of completion of request. It could have one of the following status values:</p><ul><li><code>success</code>: The extension has been successfully applied.</li><li><code>failed</code>: The extension has failed and will not be retried.</li><li><code>pending</code>: The extension will finish processing in some time. On completion, the final status (success / failed) will be sent to the <code>webhookUrl</code> provided.</li></ul><p>If no extension was requested, then this parameter is not returned.</p> |

## Examples

## Examples

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

### Understanding API usage

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X PATCH "https://api.imagekit.io/v1/files/:fileId/details" \
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
    public_key='your_public_api_key',
    private_key='your_private_api_key',
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

### Applying extensions

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
# Example of using the google-auto-tagging extension
curl -X PATCH "https://api.imagekit.io/v1/files/fileId/details" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "extensions": [
        {
            "name": "google-auto-tagging",
            "maxTags": 5,
            "minConfidence": 95
        }
    ]
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
    extensions: [
        {
            name: "google-auto-tagging",
            maxTags: 5,
            minConfidence: 95
        }
    ]
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

updated_detail = imagekit.update_file_details(
    "file_id",
    "extensions": [
        {
            "name": "google-auto-tagging",
            "maxTags": 5,
            "minConfidence": 95
        }
    ]
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

$updateFileDetails = $imageKit->updateFileDetails("file_id", array("extensions" => [array("name" => "google-auto-tagging", "maxTags" => 5, "minConfidence" => 95)]));

echo("Updated detail : " . json_encode($updateFileDetails));
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
updated_detail = imagekitio.update_file_details(
    "file_id",
    {
        "extensions": [
            {
                "name": "google-auto-tagging",
                "maxTags": 5,
                "minConfidence": 95
            }
        ]
    }
)
```
{% endtab %}
{% endtabs %}

#### Response

```javascript
/*
    "success" status for google-auto-tagging extension having
    AITags field synchronously populated.
*/
{
    "fileId" : "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": null,
    "AITags" [
        {
            "name": "saree",
            "confidence": 96.2837328,
            "source": "google-auto-tagging"
        },
        {
            "name": "traditional clothing",
            "confidence": 98.3732228,
            "source": "google-auto-tagging"
        },
        {
            "name": "women's wear",
            "confidence": 97.7233283,
            "source": "google-auto-tagging"
        },
        {
            "name": "ethnic",
            "confidence": 99.9928828,
            "source": "google-auto-tagging"
        }
    ]
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
    "updatedAt": "2019-08-24T06:14:41.313Z",
    "extensionStatus": {
        "google-auto-tagging": "success"
    }
}
```
