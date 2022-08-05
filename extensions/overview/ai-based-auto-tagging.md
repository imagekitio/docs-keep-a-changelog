---
description: Automatically categorize and add tags to your images
---

# AI-based auto-tagging

You can add AI-generated tags (AITags) to your media library images using ImageKit's [update](../../api-reference/media-api/update-file-details.md) and [upload](../../media-library/overview/upload-files.md) APIs. ImageKit leverages powerful label detection APIs by [Google Cloud Vision](https://cloud.google.com/vision/docs/labels) and [Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/labels-detect-labels-image.html) provided through its extensions to automatically detect and add tags to your images.

### How to use auto-tagging?

1. **While uploading a new image:**\
   During an [upload](../../media-library/overview/upload-files.md) API call, you must specify in the `extensions` parameter the `name` of the auto-tagging extension(s) to be used along with its `minConfidence` and `maxTags` parameters.\

2. **While updating an already existing image:**\
   You can add AITags to an existing image through an [update](../../api-reference/media-api/update-file-details.md) API call. You must specify in the `extensions` parameter the `name` of the auto-tagging extension(s) to be used along with its `minConfidence` and `maxTags` parameters.

### Extensions parameter for auto-tagging

The `extensions` parameter of both upload and update APIs takes an array of objects specifying the extensions to be used along with their parameters. Each object in this array is a self-contained block containing information about one single extension. This information includes the name of the extension and other parameters associated with that extension. The object for an auto-tagging extension will have the following fields:

| Parameter name    | Type   | Required | Description                                                                                                                                                 |
| ----------------- | ------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **name**          | string | Yes      | `google-auto-tagging` or `aws-auto-tagging`                                                                                                                 |
| **minConfidence** | number | Yes      | Tags with a confidence value below this value will be discarded. Valid values are between 0 and 100.                                                        |
| **maxTags**       | number | Yes      | Maximum number of tags to attach. If `maxTags` is 5, then only the top five tags returned from the API will be attached. Valid values are between 0 and 30. |

Here's an example of an `extensions` parameter formation that will result in the execution of two different extensions -  auto-tagging using [Google Cloud Vision](https://cloud.google.com/vision/docs/labels) and [Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/labels-detect-labels-image.html):

```javascript
// Request body for an update/upload API call
{
    /*
    ...rest of the update/upload request parameters
    */
    "extensions": [
        {
            "name": "aws-auto-tagging",
            "minConfidence": 80, // only tags with a confidence value higher than 80% will be attached
            "maxTags": 10 // a maximum of 10 tags from aws will be attached
        },
        {
            "name": "google-auto-tagging",
            "minConfidence": 70, // only tags with a confidence value higher than 70% will be attached
            "maxTags": 10 // a maximum of 10 tags from google will be attached
        }
    ]
}
```

### Asynchronous behavior

Auto-tagging extensions may execute asynchronously, i.e., after your main update/upload call has successfully completed and you have received the response for it. However, auto-tagging extensions are first tried to be performed in sync. If performed synchronously, then the response of your upload/update API call will include the `AITags` field containing the newly generated tags. However, there is no guarantee that any extension will be performed synchronously. You can inspect the response field `extensionStatus` to identify the status of each extension in your request.

### Response structure

If an update/upload API call includes a valid `extensions` field array with a non-zero length, then the response will include an `extensionStatus` field object, which will contain the status of each extension at the time of completion of the update/upload request. For auto-tagging extensions, the status has three possible values:

* `success`: The extension has been successfully applied.
* `failed`: The extension has failed and will not be retried.
* `pending`: The extension will finish processing in some time. On completion, the final status (success/failure) will be sent to the `webhookUrl` provided.

The response for an API call with the above request body can look like this

```javascript
// Response body for an update/upload API call
{
    /*
    ...rest of the update/upload response fields
    */
    "AITags": [
        {
            "name": "Shirt",
            "confidence": 90.12,
            "source": "google-auto-tagging"
        },
        /* ... more googleVision tags ... */
    ],
    "extensionStatus": {
        "google-auto-tagging": "success",
        "aws-auto-tagging": "pending"
    }
}
```

The `AITags` field is populated only because the `google-auto-tagging` extension executed synchronously i.e. it received a `success`response. The `aws-auto-tagging`extension has not yet completed execution and hence its tags are not included in this response.

### Webhooks

You can include a `webhookUrl` parameter in your update/upload API calls. The final status of extensions after they have completed execution will be delivered to this endpoint as a POST request. The request body sent to this endpoint will look like the examples below.

To learn more about how webhooks works, refer [this](./#webhooks).

```javascript
// A SUCCESS webhook request for a google auto-tagging extension
{
    "service": "google-auto-tagging",
    "status": "SUCCESS",
    "fileObject": {
        "fileId": "5effaa5662679b5af2c58829",
        "AITags": [
            {
                "name": "Shirt",
                "confidence": 90.12,
                "source": "google-auto-tagging"
            },
            /* ... more google-auto-tagging tags ... */
        ],    
        /*
        ... rest of the fields in a file object
        */
    },
}
```

```javascript
// A FAILURE webhook request for an aws auto-tagging extension
{
    "service": "aws-auto-tagging",
    "status": "FAILURE",
    "fileId": "5effaa5662679b5af2c58829",
    "error": {
        /* error description fields */
    }
}
```

### Examples

Let's apply the `google-auto-tagging` extension to the below image:

![](../../.gitbook/assets/teddy-bear-with-love-1244583-1918x1278.jpg)

### Using upload API to add AITags

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=@/Users/username/Desktop/cafe_picture.jpg;type=image/jpg' \
-F 'fileName=my_file_name.jpg'
-F 'extensions="[
  {
      \"name\": \"google-auto-tagging\",
      \"minConfidence\": 50,
      \"maxTags\": 5
  },
  {
      \"name\": \"aws-auto-tagging\",
      \"minConfidence\": 50,
      \"maxTags\": 5
  },
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
        extension.NewAutoTag(extension.GoogleAutoTag, 50, 5),
        extension.NewAutoTag(extension.AwsAutoTag, 50, 5),
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
    "AITags": [
        {
            "name": "Teddy Bear",
            "confidence": 98.52,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Toy",
            "confidence": 98.52,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Sweets",
            "confidence": 77.84,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Heart",
            "confidence": 61.19,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Plush",
            "confidence": 56.64,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Toy",
            "confidence": 91.73,
            "source": "google-auto-tagging"
        },
        {
            "name": "Teddy bear",
            "confidence": 83.52,
            "source": "google-auto-tagging"
        },
        {
            "name": "Red",
            "confidence": 81.77,
            "source": "google-auto-tagging"
        },
        {
            "name": "Stuffed toy",
            "confidence": 76.25,
            "source": "google-auto-tagging"
        },
        {
            "name": "Art",
            "confidence": 72.83,
            "source": "google-auto-tagging"
        }
    ],
    "fileType": "image",
    "extensionStatus": {
        "google-auto-tagging": "success",
        "aws-auto-tagging": "success"

    }
}
```

### Using update API to add AITags

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X PATCH "https://api.imagekit.io/v1/files/:fileId/details" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "extensions": [ 
         { 
             "name": "google-auto-tagging",
             "minConfidence": 50, 
             "maxTags": 5
         },
          { 
             "name": "aws-auto-tagging",
             "minConfidence": 50, 
             "maxTags": 5
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
            minConfidence: 50
        },
        {
            name: "aws-auto-tagging",
            maxTags: 5,
            minConfidence: 50
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
            "minConfidence": 50
        },
        {
            "name": "aws-auto-tagging",
            "maxTags": 5,
            "minConfidence": 50
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
            "name" => "google-auto-tagging",
            "maxTags" => 5,
            "minConfidence" => 50
        ],
        [
            "name" => "aws-auto-tagging",
            "maxTags" => 5,
            "minConfidence" => 50
        ]
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
          "name": "google-auto-tagging",
          "maxTags": 5,
          "minConfidence": 50
       },
       {
          "name": "aws-auto-tagging",
          "maxTags": 5,
          "minConfidence": 50
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
        extension.NewAutoTag(extension.GoogleAutoTag, 50, 5),
        extension.NewAutoTag(extension.AwsAutoTag, 50, 5),
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
    "AITags": [
        {
            "name": "Teddy Bear",
            "confidence": 98.52,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Toy",
            "confidence": 98.52,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Sweets",
            "confidence": 77.84,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Heart",
            "confidence": 61.19,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Plush",
            "confidence": 56.64,
            "source": "aws-auto-tagging"
        },
        {
            "name": "Toy",
            "confidence": 91.73,
            "source": "google-auto-tagging"
        },
        {
            "name": "Teddy bear",
            "confidence": 83.52,
            "source": "google-auto-tagging"
        },
        {
            "name": "Red",
            "confidence": 81.77,
            "source": "google-auto-tagging"
        },
        {
            "name": "Stuffed toy",
            "confidence": 76.25,
            "source": "google-auto-tagging"
        },
        {
            "name": "Art",
            "confidence": 72.83,
            "source": "google-auto-tagging"
        }
    ],
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
        "google-auto-tagging": "success",
        "aws-auto-tagging": "success"
    }
}
```

####

####
