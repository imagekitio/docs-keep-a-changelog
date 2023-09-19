# Server side file upload

You can upload files to the ImageKit.io media library from your server-side using [private API key authentication](../api-introduction/api-keys.md#private-key).

{% hint style="info" %}
**File size limit**\
The maximum upload file size is limited to 25MB on the free plan. On paid plan, this limit is 300MB for video files.

**Version limit**\
A file can have a maximum of 100 versions.
{% endhint %}

## Endpoint

| Method | Endpoint                                                                                         |
| ------ | ------------------------------------------------------------------------------------------------ |
| POST   | [https://upload.imagekit.io/api/v1/files/upload](https://upload.imagekit.io/api/v1/files/upload) |

## Request structure (multipart/form-data)

| Parameter                                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>file</strong><br>required<br></p>                   | <p>This field accepts three kinds of values:<br><br>- <code>binary</code> - You can send the content of the file as binary. This is used when a file is being uploaded from the browser.<br>- <code>base64</code> - Base64 encoded string of file content.<br>- <code>url</code> - URL of the file from where to download the content before uploading. For example - <code>https://www.example.com/rest-of-the-image-path.jpg</code>.</p><p><br><strong>Note:</strong> When passing a URL in the file parameter, please ensure that our servers can access the URL. In case ImageKit is unable to download the file from the specified URL, a <code>400</code> error response is returned. In addition to this, the file download request is aborted if response headers are not received in 8 seconds. This will also result in a <code>400</code> error.</p> |
| <p><strong>fileName</strong><br>required</p>                   | <p>The name with which the file has to be uploaded.</p><p><br>The file name can contain:<br><br>- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code> (including unicode letters, marks, and numerals in other languages)</p><p>- Special Characters: <code>.</code> <em>and <code>-</code></em></p><p>Any other character including space will be replaced by <code>_</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><strong>useUniqueFileName</strong><br>optional</p>          | <p>Whether to use a unique filename for this file or not.<br><br>- Accepts <code>true</code> or <code>false</code>.<br>- If set <code>true</code>, ImageKit.io will add a unique suffix to the filename parameter to get a unique filename.<br>- If set <code>false</code>, then the image is uploaded with the provided filename parameter, and any existing file with the same name is replaced.<br><br><strong>Default value</strong> - <code>true</code></p>                                                                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>tags</strong><br>optional<br></p>                   | <p>Set the tags while uploading the file.<br><br>- Comma-separated value of tags in the format <code>tag1,tag2,tag3</code>. For example - <code>t-shirt,round-neck,men</code><br>- The maximum length of all characters should not exceed 500.<br>- <code>%</code> is not allowed.<br>- If this field is not specified and the file is overwritten then the tags will be removed.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| <p><strong>folder</strong><br>optional<br></p>                 | <p>The folder path (e.g. <code>/images/folder/</code>) in which the image has to be uploaded. If the folder(s) didn't exist before, a new folder(s) is created. The nesting of folders can be at most 50 levels deep.<br><br>The folder name can contain:<br><br>- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code> (including unicode letters, marks, and numerals in other languages)</p><p>- Special Characters: <code>/</code> <code>_</code> and <code>-</code><br>- Using multiple <code>/</code> creates a nested folder.<br><br><strong>Default value</strong> - /</p>                                                                                                                                                                                                                                                                                                         |
| <p><strong>isPrivateFile</strong><br>optional<br></p>          | <p>Whether to mark the file as private or not. This is only relevant for image type files.<br><br>- Accepts <code>true</code> or <code>false</code>.<br>- If set <code>true</code>, the file is marked as private which restricts access to the original image URL and unnamed image transformations without signed URLs. Without the signed URL, only named transformations work on private images<br><br><strong>Default value</strong> - <code>false</code></p>                                                                                                                                                                                                                                                                                                                                                                                              |
| <p><strong>isPublished</strong><br>optional<br></p>          | <p>Whether to upload file as published or not.<br><br>- Accepts <code>true</code> or <code>false</code>.<br>- If set <code>false</code>, the file is marked as unpublished, which restricts access to the file only via the media library. Files in [draft or unpublished](../../media-library/overview/draft-assets.md) state can only be publicly accessed after being published.<br>- The option to upload in draft state is only available in custom enterprise pricing plans.<br><br><strong>Default value</strong> - <code>true</code></p>                                                                                                                                                                                                                                                                                                                                                                                              |
| <p><strong>customCoordinates</strong><br>optional<br></p>      | <p>Define an important area in the image. This is only relevant for image type files.<br></p><ul><li>To be passed as a string with the x and y coordinates of the top-left corner, and width and height of the area of interest in the format <code>x,y,width,height</code>. For example - <code>10,10,100,100</code></li><li>Can be used with <code>fo-custom</code>transformation.</li><li>If this field is not specified and the file is overwritten, then customCoordinates will be removed.</li></ul>                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>responseFields</strong><br><strong></strong>optional<br></p>                     | Comma-separated values of the fields that you want the API to return in the response. For example, set the value of this field to `tags,customCoordinates,isPrivateFile` to get the value of `tags`, `customCoordinates`, `isPublished` and `isPrivateFile` in the response. Accepts combination of `tags`, `customCoordinates`, `isPrivateFile`, `embeddedMetadata`, `customMetadata`, and `metadata`.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>extensions</strong></p><p>optional</p>              | <p>Stringified JSON object with an array of extensions to be applied to the image.</p><p>For reference about extensions <a href="../../extensions/overview/">read here</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>webhookUrl</strong></p><p>optional</p>              | The final status of pending extensions will be sent to this URL. To learn more about how ImageKit uses webhooks, [refer here](../../extensions/overview/#webhooks).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>overwriteFile</strong></p><p>optional</p>           | Default is `true`. If `overwriteFile` is set to `false` and `useUniqueFileName` is also `false`, and a file already exists at the exact location, upload API will return an error immediately.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>overwriteAITags</strong></p><p>optional</p>         | Default is `true`. If set to `true` and a file already exists at the exact location, its `AITags` will be removed. Set `overwriteAITags` to `false` to preserve `AITags`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p><strong>overwriteTags</strong></p><p>optional</p>           | Default is `true`. If the request does not have `tags` , `overwriteTags` is set to `true` and a file already exists at the exact location, exiting `tags` will be removed. In case the request body has `tags`, setting `overwriteTags` to `false` has no effect and request's `tags` are set on the asset.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>overwriteCustomMetadata</strong></p><p>optional</p> | Default is `true`. If the request does not have `customMetadata` , `overwriteCustomMetadata` is set to `true` and a file already exists at the exact location, exiting `customMetadata` will be removed. In case the request body has `customMetadata`, setting `overwriteCustomMetadata` to `false` has no effect and request's `customMetadata` is set on the asset.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>customMetadata</strong></p><p>optional</p>                                         |  Stringified JSON key-value data to be associated with the asset. Checkout `overwriteCustomMetadata` parameter to understand default behaviour. Before setting any custom metadata on an asset you have to create the field using [custom metadata fields API](../custom-metadata-fields-api/).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

### Response code and structure (JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On successful upload, you will receive a `200` status code with uploaded file details in a JSON-encoded response body.

```javascript
{
    "fileId": "598821f949c0a938d57563bd",
    "name": "file1.jpg",
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnailUrl": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "height": 300,
    "width": 200,
    "size": 83622,
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
    "customMetadata": {
        brand: "Nike",
        color: "red"
    },
    "embeddedMetadata": {
        Title: "The Title (ref2019.1)",
        Description: "The description aka caption (ref2019.1)",
        State: "Province/State(Core)(ref2019.1)",
        Copyright: "Copyright (Notice) 2019.1 IPTC - www.iptc.org  (ref2019.1)"
    },
    "extensionStatus": {
        "google-auto-tagging": "success",
        "aws-auto-tagging": "pending"
    },
    "fileType": "image"
}
```

### Understanding response

The JSON-encoded response contains details of the uploaded file which can have the following properties:

| Property name     | Description                                                                                                                                                                                                                             |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fileId            | Unique `fileId`. Store this `fileld` in your database, as this will be used to perform update action on this file                                                                                                                       |
| name              | Name of the file.                                                                                                                                                                                                                       |
| filePath          | The relative path of the file. In the case of an image, you can use this path to construct different transformations.                                                                                                                   |
| tags              | The array of tags associated with the image. If no tags are set, it will be `null`.                                                                                                                                                     |
| AITags            | Array of `AITags` associated with the image. If no `AITags` are set, it will be `null`. These tags can be added using the `google-auto-tagging` or `aws-auto-tagging` [extensions](../../extensions/overview/ai-based-auto-tagging.md). |
| versionInfo       | An object containing the file or file version's <code>id</code> (versionId) and <code>name</code>. |
| isPrivateFile     | Is the file marked as private. It can be either `true` or `false`.                                                                                                                                                                      |
| customCoordinates | <p>Value of custom coordinates associated with the image in the format <code>x,y,width,height</code>. If customCoordinates are not defined, then it is <code>null</code>.<br></p>                                                       |
| url               | A publicly accessible URL of the file.                                                                                                                                                                                                  |
| thumbnailUrl      | In the case of an image, a small thumbnail URL.                                                                                                                                                                                         |
| fileType          | The type of file could be either `image` or `non-image`.                                                                                                                                                                                |
| height            | Height of the media file in pixels (Only for images and videos)                                                                                                                                                                         |
| width             | Width of the media file in pixels (Only for images and videos)                                                                                                                                                                          |
| size              | Size of the file in Bytes                                                                                                                                                                                                               |
| bitRate           | Bitrate of the media file (Only for videos)                                                                                                                                                                                             |
| videoCodec        | Video codec of the first stream for the media file (Only for videos)                                                                                                                                                                    |
| audioCodec        | Audio codec of the first stream for the media file (Only for videos)                                                                                                                                                                    |
| duration          | Duration of the media file content in seconds (Only for videos)                                                                                                                                                                         |
| customMetadata    | A key-value data associated with the asset. Use `responseField` in API request to get `customMetadata` in the upload API response. Before setting any custom metadata on an asset, you have to create the field using [custom metadata fields API](../custom-metadata-fields-api/).                                                                                                                                                                                                                         |
| embeddedMetadata  | Consolidated embedded metadata associated with the file. It includes `exif`, `iptc`, and `xmp` data. Use `responseField` in API request to get `embeddedMetadata` in the upload API response.                                           |
| metadata          | Metadata associated with the file in legacy format.                                                                                                                                                                                     |
| extensionStatus   | <p>Extension names with their processing status at the time of completion of the request. It could have one of the following status values:</p><ul><li><code>success</code>: The extension has been successfully applied.</li><li><code>failed</code>: The extension has failed and will not be retried.</li><li><code>pending</code>: The extension will finish processing in some time. On completion, the final status (success / failed) will be sent to the <code>webhookUrl</code> provided.</li></ul><p>If no extension was requested, then this parameter is not returned.</p>                                                                                                                                                                 |

## Examples

Here are some example requests and responses to understand the API usage.

### Uploading file from file system

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=@/Users/username/Desktop/my_file_name.jpg;type=image/jpg' \
-F 'fileName=my_file_name.jpg'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");
var fs = require('fs');

var imagekit = new ImageKit({
    publicKey : "your_public_key",
    privateKey : "your_private_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

fs.readFile('image.jpg', function(err, data) {
  if (err) throw err; // Fail if the file can't be read.
  imagekit.upload({
    file : data, //required
    fileName : "my_file_name.jpg", //required
    tags: ["tag1", "tag2"]
  }, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
  });
});
```
{% endtab %}

{% tab title="Python" %}
```python
import base64
import os
import sys
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your_public_key',
    private_key='your_private_key',
    url_endpoint = 'your_url_endpoint'
)

upload = imagekit.upload(
    file=open("image.jpg", "rb"),
    file_name="my_file_name.jpg",
    options=UploadFileRequestOptions(
        tags = ["tag1", "tag2"]
    )
)

print("Upload binary", upload)

# Raw Response
print(upload.response_metadata.raw)

# print that uploaded file's ID
print(upload.file_id)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_key";
$your_private_key = "your_private_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";
$sample_file_path = "/sample.jpg";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

// Upload Image - Binary
$uploadFile = $imageKit->uploadFile([
    "file" => fopen(__DIR__."/image.jpg", "r"),
    "fileName" => "my_file_name.jpg",
    "tags" => ["tag1", "tag2"]
]);

echo ("Upload binary file : " . json_encode($uploadFile));
```
{% endtab %}

{% tab title="Java" %}
```java
byte[] bytes= Files.readAllBytes(Paths.get("/path/to/file.jpg"));
FileCreateRequest fileCreateRequest =new FileCreateRequest(bytes, "sample_image.jpg");
fileCreateRequest.setUseUniqueFileName(false);
Result result = ImageKit.getInstance().upload(fileCreateRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
file = open("sample.jpg", "rb")
upload = imagekitio.upload_file(file, "my_file_name.jpg", {
    tags: %w[tag1 tag2]
})
```
{% endtab %}

{% tab title="Go" %}
```go
ik, err := ImageKit.New()

const base64Image = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7"
resp, err := ik.Upload.Upload(ctx, base64Image, uploader.UploadParam{})
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
byte[] bytes = File.ReadAllBytes("/path/to/file.jpg");
FileCreateRequest ob = new FileCreateRequest
    {
        file = bytes,
        fileName = "file_name1.jpg" 
    };
Result resp2 = imagekit.Upload(ob);
```
{% endtab %}

{% endtabs %}

### Uploading base64 encoded file with some tags

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=iVBORw0KGgoAAAAN' \
-F 'fileName=my_file_name.jpg' \
-F 'tags=tag1,tag2'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");
var fs = require('fs');

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

var base64Img = "iVBORw0KGgoAAAAN";

imagekit.upload({
    file : base64Img, //required
    fileName : "my_file_name.jpg",   //required
    tags: ["tag1","tag2"]
}, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
import base64
import os
import sys
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your public_key',
    private_key='your private_key',
    url_endpoint = 'your url_endpoint'
)

with open("image.jpg", mode="rb") as img:
    imgstr = base64.b64encode(img.read())

upload = imagekit.upload(
    file=imgstr,
    file_name="my_file_name.jpg",
    options=UploadFileRequestOptions(
            response_fields = ["is_private_file", "custom_metadata", "tags"],
            is_private_file = False,
            tags = ["tag1", "tag2"],
            webhook_url = "url",
            overwrite_file = False,
            overwrite_ai_tags = False,
            overwrite_tags = False,
            overwrite_custom_metadata = True,
            custom_metadata = {"test": 11})
    ),
)

print("Upload base64", upload)

# Raw Response
print(upload.response_metadata.raw)

# print that uploaded file's ID
print(upload.file_id)

# print that uploaded file's version ID
print(upload.version_info.id)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_key";
$your_private_key = "your_private_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";
$sample_file_path = "/sample.jpg";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$img = file_get_contents(__DIR__."/image.jpg");

// Encode the image string data into base64
$base64Img = base64_encode($img);


// Upload Image - base64
$uploadFile = $imageKit->uploadFile([
    "file" => $base64Img,
    "fileName" => "my_file_name.jpg",
    "tags" => ["tag1", "tag2"]
]);

echo ("Upload base64" . json_encode($uploadFile));
```
{% endtab %}

{% tab title="Java" %}
```java
FileCreateRequest fileCreateRequest =new FileCreateRequest(base64, "sample_base64_image.jpg");
Result result = ImageKit.getInstance().upload(fileCreateRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")

image64 = Base64.encode64(File.open("sample.jpg", "rb").read)

upload = imagekitio.upload_file(
    file: image64,
    file_name: "my_file_name.jpg",
    tags: %w[tag1 tag2]
 )
```
{% endtab %}

{% tab title="Go" %}
```go
ik, err := ImageKit.New()
const base64Image = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7"

resp, err := ik.Upload.Upload(ctx, base64Image, uploader.UploadParam{
    FileName: "my_file_name.jpg",
    Tags: "tag1,tag2",
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
var base64ImageRepresentation = "iVBORw0KGgoAAAAN";
FileCreateRequest ob2 = new FileCreateRequest
    {
        file = base64ImageRepresentation,
        fileName = Guid.NewGuid().ToString(),
    };
List<string> tags = new List<string>
    {
        "tags1",
        "tags2"               
    };
ob.tags = tags;
Result resp = imagekit.Upload(ob2);
```
{% endtab %}

{% endtabs %}

### Uploading file via URL

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=https://imagekit.io/image.jpg' \
-F 'fileName=my_file_name.jpg'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");
var fs = require('fs');

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

imagekit.upload({
    file : "https://imagekit.io/image.jpg", //required
    fileName : "my_file_name.jpg" //required
}, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
import base64
import os
import sys
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your public_key',
    private_key='your private_key',
    url_endpoint = 'your url_endpoint'
)

with open("image.jpg", mode="rb") as img:
    imgstr = base64.b64encode(img.read())

upload = imagekit.upload(
    file="https://imagekit.io/image.jpg",
    file_name="my_file_name.jpg",
    options=UploadFileRequestOptions(),
)

print("Upload url", upload)

# Raw Response
print(upload.response_metadata.raw)

# print that uploaded file's ID
print(upload.file_id)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_key";
$your_private_key = "your_private_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";
$sample_file_path = "/sample.jpg";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

// Upload Image - URL
$uploadFile = $imageKit->uploadFile([
    "file" => "https://imagekit.io/image.jpg",
    "fileName" => "my_file_name.jpg"
]);

echo ("Upload URL" . json_encode($uploadFile));
```
{% endtab %}

{% tab title="Java" %}
```java
FileCreateRequest fileCreateRequest = new FileCreateRequest("image_url", "my_file_name.jpg");
Result result = ImageKit.getInstance().upload(fileCreateRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")

upload = imagekitio.upload_file(
    file: "image_url",
    file_name: "my_file_name.jpg"
)
```
{% endtab %}

{% tab title="Go" %}
```go
url := "https://imagekit.io/image.jpg"
resp, err := ik.Upload.Upload(ctx, url, uploader.UploadParam{
    FileName: "my_file_name.jpg",
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
FileCreateRequest request = new FileCreateRequest
    {
       file = "image_url",
       fileName = "file_name.jpg"
    };
Result resp1 = imagekit.Upload(request);
```
{% endtab %}

{% endtabs %}

### Setting custom metadata during upload

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg' \
-F 'fileName=women_in_red.jpg' \
-F 'customMetadata={"brand":"Nike", "color":"red"}'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");
var fs = require('fs');

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

imagekit.upload({
    file : "https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",
    fileName : "women_in_red.jpg",
    customMetadata : {
        "brand": "Nike",
        "color": "red"
    }
}, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
import base64
import os
import sys
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your public_key',
    private_key='your private_key',
    url_endpoint = 'your url_endpoint'
)

with open("image.jpg", mode="rb") as img:
    imgstr = base64.b64encode(img.read())

upload = imagekit.upload(
    file="https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",
    file_name="women_in_red.jpg",
    options=UploadFileRequestOptions(
        custom_metadata = {"brand":"Nike", "color":"red"}
    ),
)

print("Upload url", upload)

# Raw Response
print(upload.response_metadata.raw)

# print that uploaded file's ID
print(upload.file_id)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_key";
$your_private_key = "your_private_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";
$sample_file_path = "/sample.jpg";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

// Upload Image - URL
$uploadFile = $imageKit->uploadFile([
    "file" => "https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",
    "fileName" => "women_in_red.jpg",
    "customMetadata" => [
        "brand" => "Nike",
        "color" => "red",
    ]
]);

echo ("Upload URL" . json_encode($uploadFile));
```
{% endtab %}

{% tab title="Java" %}
```java
FileCreateRequest fileCreateRequest = new FileCreateRequest("https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",  "women_in_red.jpg");
JsonObject jsonObjectCustomMetadata = new JsonObject();
jsonObjectCustomMetadata.addProperty("brand", "Nike");
jsonObjectCustomMetadata.addProperty("color", "red");
fileCreateRequest.setCustomMetadata(jsonObjectCustomMetadata);
Result result=ImageKit.getInstance().upload(fileCreateRequest);

System.out.println(result);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
upload = imagekitio.upload_file(
    file: "https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",
    file_name: "women_in_red.jpg",
    custom_metadata: {
      "brand": "Nike",
      "color": "red"
    }
)
```
{% endtab %}

{% tab title="Go" %}
```go
url := "https://imagekit.io/image.jpg"
resp, err := ik.Upload.Upload(ctx, url, uploader.UploadParam{
    FileName: "women_in_red.jpg",
    CustomMetadata: `{"brand":"Nike", "color":"red"}`,
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
var base64ImageRepresentation = "iVBORw0KGgoAAAAN";
FileCreateRequest ob2 = new FileCreateRequest
    {
        file = base64ImageRepresentation,
        fileName = Guid.NewGuid().ToString()
    };
 Hashtable model = new Hashtable
    {
        { "brand", "Nike" },
        { "color", "red" }
    };
ob2.customMetadata = model;
Result resp = imagekit.Upload(ob2);
```
{% endtab %}

{% endtabs %}

### Applying extensions while uploading

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://upload.imagekit.io/api/v1/files/upload" \
-u your_private_api_key: \
-F 'file=@/Users/username/Desktop/my_file_name.jpg' \
-F 'fileName=my_file_name.jpg' \
-F 'extensions=[{"name":"remove-bg","options":{"add_shadow":true,"bg_colour":"green"}},{"name":"google-auto-tagging","maxTags":5,"minConfidence":95}]'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");
var fs = require('fs');

var imagekit = new ImageKit({
    publicKey : "your_public_key",
    privateKey : "your_private_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

fs.readFile('image.jpg', function(err, data) {
  if (err) throw err; // Fail if the file can't be read.
  imagekit.upload({
    file : data, //required
    fileName : "my_file_name.jpg", //required
    extensions: '[
        {
          "name": "remove-bg",
          "options": {
            "add_shadow": true,
            "bg_colour": "green"
          }
        },
        {
            "name": "google-auto-tagging",
            "maxTags": 5,
            "minConfidence": 95
        }
    ]'
  }, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
  });
});
```
{% endtab %}

{% tab title="Python" %}
```python
import base64
import os
import sys
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your public_key',
    private_key='your private_key',
    url_endpoint = 'your url_endpoint'
)

with open("image.jpg", mode="rb") as img:
    imgstr = base64.b64encode(img.read())

upload = imagekit.upload(
    file="https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",
    file_name="women_in_red.jpg",
    options=UploadFileRequestOptions(
        extensions = [{"name": "remove-bg", "options": {"add_shadow": True, "bg_color": "pink"}},
                {"name": "google-auto-tagging", "minConfidence": 80, "maxTags": 10}]
    ),
)

print("Upload url", upload)

# Raw Response
print(upload.response_metadata.raw)

# print that uploaded file's ID
print(upload.file_id)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_key";
$your_private_key = "your_private_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";
$sample_file_path = "/sample.jpg";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);


// Upload Image - URL
$uploadFile = $imageKit->upload([
    "file" => "https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",
    "fileName" => "women_in_red.jpg",
    "extensions" => [
        [
            "name" => "remove-bg",
            "options" => [
                "add_shadow" => true,
                "bg_colour" => "green"
            ]
        ],
        [
            "name" => "google-auto-tagging",
            "maxTags" => 5,
            "minConfidence" => 95
        ]
    ],
]);

echo ("Upload URL" . json_encode($uploadFile));
```
{% endtab %}

{% tab title="Java" %}
```java

FileCreateRequest fileCreateRequest =new FileCreateRequest("https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",  "women_in_red.jpg");
JsonObject optionsInnerObject = new JsonObject();
optionsInnerObject.addProperty("add_shadow", true);
optionsInnerObject.addProperty("bg_colour", "green");
JsonObject innerObject1 = new JsonObject();
innerObject1.addProperty("name", "remove-bg");
innerObject1.add("options", optionsInnerObject);
JsonObject innerObject2 = new JsonObject();
innerObject2.addProperty("name", "google-auto-tagging");
innerObject2.addProperty("minConfidence", 5);
innerObject2.addProperty("maxTags", 95);
JsonArray jsonArray = new JsonArray();
jsonArray.add(innerObject1);
jsonArray.add(innerObject2);
fileCreateRequest.setExtensions(jsonArray);
Result result=ImageKit.getInstance().upload(fileCreateRequest);

System.out.println(result);
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
        extension.NewAutoTag(extension.AwsAutoTag, 0, 10),
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
var base64ImageRepresentation = "iVBORw0KGgoAAAAN";
FileCreateRequest ob2 = new FileCreateRequest
    {
        file = base64ImageRepresentation,
        fileName = Guid.NewGuid().ToString()
    };
List<Extension> ext = new List<Extension>();
BackGroundImage bck1 = new BackGroundImage
    {
        name = "remove-bg",
        options = new options()
        { 
            add_shadow = true, semitransparency = false, bg_image_url = "http://www.google.com/images/logos/ps_logo2.png" 
        }
    };
AutoTags autoTags = new AutoTags
    {
        name = "google-auto-tagging",
        maxTags = 5,
        minConfidence = 95  
    };
ext.Add(bck1);
ext.Add(autoTags);
ob2.extensions = ext;
Result resp = imagekit.Upload(ob2);
```
{% endtab %}

{% endtabs %}
