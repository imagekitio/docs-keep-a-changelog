# Update file details

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/details" method="patch" summary="Update file details API" %}
{% swagger-description %}
Update file details such as `tags`, `customCoordinates` attributes, remove existing `AITags` and apply [extensions](../../extensions/overview/) using update file detail API. This operation can only be performed on the current version of the file.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" required="false" %}
The unique fileId of the uploaded file. `fileId`is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="false" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="removeAITags" type="array" required="false" %}
An array of AITags associated with the file that you want to remove e.g. `["car", "vehicle", "motorsports"]`. If you want to remove all `AITags` associated with the file, send a string - `"all"`.

**Note:** Remove operation for`AITags`executes before any of the `extensions`are processed.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="webhookUrl" type="string" required="false" %}
The final status of pending extensions will be sent to this URL. To learn more about how webhooks works, refer [this](../../extensions/overview/#webhooks).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="extensions" type="array" required="false" %}
Array of extensions to be processed on the asset. For reference about extensions refer [this](../../extensions/overview/). Note: [Remove.bg](../../extensions/overview/background-removal.md) extension creates a new file version which will also have the updated file details.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="tags" type="array" required="false" %}
An array of tags associated with the file e.g. `["tag1", "tag2"]`. If you want to unset it send `null`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="customCoordinates" type="string" required="false" %}
Define an important area in the image in the format `x,y,width,height`e.g. `10,10,100,100`. If you want to unset this send `null`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="customMetadata" type="JSON" required="false" %}
A key-value data to be associated with the asset. To unset a key, send `null` value for that key. Before setting any custom metadata on an asset you have to create the field using [custom metadata fields API](../custom-metadata-fields-api/). Example - `{brand: "Nike", color: "red"}`.
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive the updated file details in the JSON-encoded response body." %}
```javascript
// This example response is after extensions are applied in the update API.
{
    "fileId" : "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt","round-neck","sale2019"],
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
    "customMetadata": {
        brand: "Nike",
        color: "red"
    },
    "extensionStatus": {
        "google-auto-tagging": "success",
        "aws-auto-tagging": "pending"
    },
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z"
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the [file object](./#file-object-structure) in JSON-encoded response body.

## Examples

### Tags and custom coordinate update

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X PATCH "https://api.imagekit.io/v1/files/:fileId/details" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "tags": [
        "tag1", "tag2"
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
    file_id="file_id",
    options=UpdateFileRequestOptions(remove_ai_tags=['remove-ai-tag-1', 'remove-ai-tag-2'],
                                         webhook_url="url",
                                         tags=["tag1", "tag2"], custom_coordinates="10,10,100,100",
                                         custom_metadata={"test": 11})
)

print("Updated detail-", updated_detail, end="\n\n")

# Raw Response
print(updated_detail.response_metadata.raw)

# print that file's id
print(updated_detail.file_id)
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

// Update File Details
$updateData = [
    "tags" => ["tag1", "tag2"],
    "customCoordinates" => "10,10,100,100"
];

$updateFileDetails = $imageKit->updateFileDetails(
    $fileId,
    $updateData
);

echo("Updated File Details : " . json_encode($updateFileDetails));
```
{% endtab %}

{% tab title="Java" %}
```java
 FileUpdateRequest fileUpdateRequest =new FileUpdateRequest("file_id");
 List<String> tags=new ArrayList<>();
 tags.add("tag1");
 tags.add("tag2");
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
        "tags": ["tag1", "tag2"],
        "custom_coordinates": "10,10,100,100"
    }
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.UpdateFile(ctx, "file_id", media.UpdateFileParam{
    Tags: []string{"tag1", "tag2"},
    CustomCoordinates: "10,10,100,100",
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
FileUpdateRequest updateob = new FileUpdateRequest
{
    fileId = "fileId",
};
List<string> updatetags = new List<string>
{
    "tag1",
    "tag2"
};
updateob.tags = updatetags;
string updatecustomCoordinates = "10,10,100,100";
updateob.customCoordinates = updatecustomCoordinates;
Result updateresp = imagekit.UpdateFileDetail(updateob);
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
curl -X PATCH "https://api.imagekit.io/v1/files/file_id/details" \
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
    file_id="file_id",
    options=UpdateFileRequestOptions(extensions = [
        {
            "name": "google-auto-tagging",
            "maxTags": 5,
            "minConfidence": 95
        }
    ])
)

print("Updated detail-", updated_detail, end="\n\n")

# Raw Response
print(updated_detail.response_metadata.raw)

# print that file's id
print(updated_detail.file_id)
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

// Update File Details
$updateData = [
    "extensions" => [
        [
            "name" => "google-auto-tagging",
            "maxTags" => 5, 
            "minConfidence" => 95
        ]
    ]
];

$updateFileDetails = $imageKit->updateFileDetails(
    $fileId,
    $updateData
);

echo("Updated File Details : " . json_encode($updateFileDetails));
```
{% endtab %}

{% tab title="Java" %}
```java
JsonObject extension = new JsonObject();
extension.addProperty("name", "google-auto-tagging");
extension.addProperty("maxTags", 5);
extension.addProperty("minConfidence", 95);
JsonArray extensionArray = new JsonArray();
extensionArray.add(extension);
fileUpdateRequest.setExtensions(extensionArray);
Result result = ImageKit.getInstance().updateFileDetail(fileUpdateRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
updated_detail = imagekitio.update_file_details(
    file_id: "file_id", #required
    extension: [
      {
        name: 'google-auto-tagging',
        maxTags: 5,
        minConfidence: 95
      }
    ]
)
```
{% endtab %}

{% tab title="Go" %}
```go
import (
    "github.com/imagekit-developer/imagekit-go/extension"
	"github.com/imagekit-developer/imagekit-go/api/uploader"
)

const base64Image = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7"

resp, err := ik.Uploader.Upload(ctx, base64Image, uploader.UploadParam{
    Extensions: []extension.IExtension{
        extension.NewAutoTag(extension.GoogleAutoTag, 95, 5),
        extension.NewRemoveBg(extension.RemoveBgOption{}),
    },
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
FileUpdateRequest updateob = new FileUpdateRequest
    {
     fileId = "fileId",
    };
List<Extension> extModel = new List<Extension>();
BackGroundImage bck = new BackGroundImage
    {
        name = "remove-bg",
        options = new options() { add_shadow = true, semitransparency = false, bg_color = "green" }
    };
extModel.Add(bck);
updateob.extensions = extModel;
Result updateresp = imagekit.UpdateFileDetail(updateob);
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
