---
description: Remove backgrounds from your images
---

# Background removal

You can remove the background from any image in your media library with the [**remove.bg**](https://www.remove.bg) third-party service using ImageKit's update and upload API. ImageKit consumes APIs provided by that service to generate the output image, and then uploads it to your media library. You can specify a few advanced parameters supported by the [remove.bg](https://www.remove.bg).

### How to use background removal?

1. **While uploading a new image:**\
   During an [upload](../../media-library/overview/upload-files.md) API call, you must specify inside the `extensions` parameter the `name` of the service to be used and optionally the`options` object. The background removal operation will begin **after** the image has been uploaded.\

2. **While updating an already existing image:**\
   You can remove the background from an existing image through an [update](../../api-reference/media-api/update-file-details.md) API call. You must specify inside the `extensions` parameter the `name` of the service to be used and optionally the `options` object. The background removal operation will begin **after** the image has been updated.

### Extensions parameter for background removal

The `extensions` parameter of both upload and update APIs takes an array of objects specifying the extensions to be used along with their parameters. Each object in this array is a self-contained block containing information about one single extension. This information includes the name of the extension and other parameters associated with that extension. The object for a background removal extension will have the following fields:

| Parameter name | Type   | Required | Valid input                                                                                                                                                                                                                                    |
| -------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **name**       | string | Yes      | `remove-bg`                                                                                                                                                                                                                                    |
| **options**    | object | No       | An object containing fields that are directly sent to the third-party service as request parameters. Example: the [remove.bg](https://remove.bg) service supports the boolean parameter `add_shadow` See below for a list of available options |

#### **Supported `options` parameters**

The options are passed to remove-bg service as it is. We support the following options:

| Parameter name       | Type    | Description                                                                                                                                                              |
| -------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **add_shadow**       | boolean | Whether to add an artificial shadow to the result (default: false). NOTE: Adding shadows is currently only supported for car photos.                                     |
| **semitransparency** | boolean | Whether to have semi-transparent regions in the result (default: true). NOTE: Semitransparency is currently only supported for car windows.                              |
| **bg_color**         | string  | Adds a solid color background. Can be a hex color code (e.g. 81d4fa, fff) or a color name (e.g. green). If this parameter is present, then `bg_image_url` must be empty. |
| **bg_image_url**     | string  | Adds a background image from a URL. If this parameter is present, then `bg_color` must be empty.                                                                         |

To learn more about what these do, refer to [remove.bg's documentation](https://www.remove.bg/api#api-reference).

Here's an example of an `extensions` parameter formation that will result in the execution of a background removal extension using the [remove.bg](https://www.remove.bg) service. Here, the `add_shadow` and `bg_color` options fields are directly sent to the [remove.bg](https://www.remove.bg) API as request body parameters.

```javascript
// Request body for an update/upload API call
{
    /*
    ...rest of the update/upload request parameters
    */
    "extensions": [
        {
            "name": "remove-bg",
            "options": {
                "add_shadow": true,
                "bg_color": "green"
            }
        }
    ]
}
```

### Asynchronous behavior

Background removal extensions **always** execute asynchronously i.e. executed after your main update/upload call has successfully completed and you have received the response for it. 

### Response structure

If an update/upload API call includes a valid `extensions` field array with a non-zero length, then the response will include an `extensionStatus` field object, which will contain the status of each extension at the time of completion of the update/upload request. For background removal extensions, the status has two possible values:

* `failed`: The extension has failed and will not be retried.
* `pending`: The extension will finish processing in some time. On completion, the final status (success/failure) will be sent to the `webhookUrl` provided

The response for an API call with the above request body might look like this

```javascript
// Response body for an update/upload API call
{
    /*
    ...rest of the update/upload response fields
    */
    "extensionStatus": {
        "remove-bg": "pending"
    }
}
```

### Webhooks

You can include a `webhookUrl` parameter in your update/upload API calls. The final status of extensions after they have completed execution will be delivered to this endpoint as a POST request. The request body sent to this endpoint will look like the examples below.

To learn more about how webhooks works, refer [this](./#webhooks).

```javascript
// A success response for a remove-bg extension
{
    "service": "remove-bg",
    "status": "SUCCESS",
    "fileObject": {
        "fileId": "5effaa5662679b5af2c58829",    
        /*
        ... rest of the fields in a file object
        */
    },
}
```

```javascript
// A failure response for a remove-bg extension
{
    "service": "remove-bg",
    "status": "FAILURE",
    "fileId": "5effaa5662679b5af2c58829",
    "error": {
        /* error description fields */
    }
}
```

### Examples

Let's apply the background removal extension to the below image:

![](../../.gitbook/assets/jose-martinez-Q76SMw8HVYk-unsplash.jpg)

### Using the upload API for background removal

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=@/Users/username/Desktop/cafe_picture.jpg;type=image/jpg' \
-F 'fileName=my_file_name.jpg'
-F 'extensions="[
   { 
      \"name\": \"remove-bg"
   }
]"'
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
        extension.NewRemoveBg(extension.RemoveBgOption{}),
    },
})

```
{% endtab %}
{% endtabs %}

#### Response

```javascript
{
    "fileId" : "598821f949c0a938d57563bd",
    "name": "file1.jpg",
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnailUrl": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "height" : 300,
    "width" : 200",
    "size" : 83622,
    "filePath": "/images/products/file1.jpg",
    "AITags": null,
    "fileType": "image",
    "extensionStatus": {
        "remove-bg": "pending"
    }
}

```

**Output**

![](<../../.gitbook/assets/Screenshot 2021-09-28 at 12.45.35 AM.png>)

### Using the update API to background removal

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X PATCH "https://api.imagekit.io/v1/files/fileId/details" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "extensions": [ 
         { 
             "name": "remove-bg"
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
             name: "remove-bg"
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
             "name": "remove-bg"
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

// Update File Details
$updateData = [
    "extensions" => [
        [
            "name" => "remove-bg",
        ],
    ],
];

$updateFileDetails = $imageKit->updateFileDetails(
    $fileId,
    $updateData
);

echo("Updated detail : " . json_encode($updateFileDetails));
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
updated_detail = imagekitio.update_file_details(
    file_id: "file_id_xyz",
    extensions: [
      {
        "name": "remove-bg",
      }
    ]
)
```
{% endtab %}

{% tab title="Go" %}
```go
import (
    "github.com/imagekit-developer/imagekit-go/extension"
	"github.com/imagekit-developer/imagekit-go/api/media"
)

resp, err := ik.Media.UpdateFile(ctx, "file_id", media.UpdateFileParam{
    Extensions: []extension.IExtension{
        extension.NewRemoveBg(extension.RemoveBgOption{}),
    },
})

```
{% endtab %}

{% endtabs %}

#### Response

```javascript
{
    "fileId": "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": null,
    "AITags": null,
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
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z",
    "extensionStatus": {
        "remove-bg": "pending"
    }
}

```

**Output Image**

![](<../../.gitbook/assets/Screenshot 2021-09-28 at 12.45.35 AM.png>)

### Using the `options` parameters for advanced features

You can add any of the [supported](background-removal.md#supported-options-parameters) `options` parameters to your `extensions` object to perform certain advanced operations along with background removal.

#### Examples

1. **bg_color**

![](<../../.gitbook/assets/Screenshot 2021-09-28 at 1.02.29 AM.png>)

    2\. **add_shadow **(only works on car images)

![](<../../.gitbook/assets/Screenshot 2021-09-28 at 1.09.41 AM.png>)

    3\. **bg_image_url**

![](<../../.gitbook/assets/Screenshot 2021-09-28 at 1.44.07 AM.png>)
