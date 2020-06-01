# Server side file upload

You can upload files to ImageKit.io media library from your server-side using [private API key authentication](../api-introduction/api-keys.md#private-key).

## Endpoint

| Method | Endpoint |
| :--- | :--- |
| POST | [https://upload.imagekit.io/api/v1/files/upload](https://upload.imagekit.io/api/v1/files/upload) |

## Request structure \(multipart/form-data\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>file</b>
        <br />required
        <br />
      </td>
      <td style="text-align:left">This field accepts three kinds of values:
        <br />
        <br />- <code>binary</code> - You can send the content of the file as binary.
        This is used when a file is being uploaded from the browser.
        <br />- <code>base64</code> - Base64 encoded string of file content.
        <br />- <code>url</code> - URL of the file from where to download the content
        before uploading. Downloading file from URL might take longer, so it is
        recommended that you pass the binary or base64 content of the file. Pass
        the full URL, for example - <code>https://www.example.com/rest-of-the-image-path.jpg</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>fileName</b>
        <br />required</td>
      <td style="text-align:left">The name with which the file has to be uploaded.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>useUniqueFileName</b>
        <br />optional</td>
      <td style="text-align:left">Whether to use a unique filename for this file or not.
        <br />
        <br />- Accepts <code>true</code> or <code>false</code>.
        <br />- If set <code>true</code>, ImageKit.io will add a unique suffix to the
        filename parameter to get a unique filename.
        <br />- If set <code>false</code>, then the image is uploaded with the provided
        filename parameter and any existing file with the same name is replaced.
        <br
        />
        <br /><b>Default value</b> - <code>true</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>tags</b>
        <br />optional
        <br />
      </td>
      <td style="text-align:left">Set the tags while uploading the file.
        <br />
        <br />- Comma-separated value of tags in format <code>tag1,tag2,tag3</code>.
        For example - <code>t-shirt,round-neck,men</code>
        <br />- The maximum length of all characters should not exceed 500.
        <br />- <code>%</code> is not allowed.
        <br />- If this field is not specified and the file is overwritten then the
        tags will be removed.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>folder</b>
        <br />optional
        <br />
      </td>
      <td style="text-align:left">The folder path (e.g. <code>/images/folder/</code>) in which the image
        has to be uploaded. If the folder(s) didn&apos;t exist before, a new folder(s)
        is created.
        <br />
        <br />The folder name can contain:
        <br />
        <br />- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code>
        <br
        />- Special Characters: <code>/``_</code> and <code>-</code>
        <br />- Using multiple <code>/</code> creates a nested folder.
        <br />
        <br /><b>Default value</b> - /</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>isPrivateFile</b>
        <br />optional
        <br />
      </td>
      <td style="text-align:left">Whether to mark the file as private or not. This is only relevant for
        image type files.
        <br />
        <br />- Accepts <code>true</code> or <code>false</code>.
        <br />- If set <code>true</code>, the file is marked as private which restricts
        access to the original image URL and unnamed image transformations without
        signed URLs. Without the signed URL, only named transformations work on
        private images
        <br />
        <br /><b>Default value</b> - <code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>customCoordinates<br /></b>optional
        <br />
      </td>
      <td style="text-align:left">
        <p>Define an important area in the image. This is only relevant for image
          type files.
          <br />
        </p>
        <ul>
          <li>To be passed as a string with the x and y coordinates of the top-left
            corner, and width and height of the area of interest in format <code>x,y,width,height</code>.
            For example - <code>10,10,100,100</code>
          </li>
          <li>Can be used with <code>fo-custom</code>transformation.</li>
          <li>If this field is not specified and the file is overwritten, then customCoordinates
            will be removed.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>responseFields</b>
        <br />optional</td>
      <td style="text-align:left">Comma-separated values of the fields that you want ImageKit.io to return
        in response.
        <br />
        <br />For example, set the value of this field to <code>tags,customCoordinates,isPrivateFile,metadata</code> to
        get value of <code>tags</code>, <code>customCoordinates</code>, <code>isPrivateFile</code> ,
        and <code>metadata</code> in the response.</td>
    </tr>
  </tbody>
</table>

## Response code and structure \(JSON\)

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On successful upload, you will receive a `200` status code with uploaded file details in a JSON-encoded response body.

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
    "tags": ["t-shirt","round-neck","sale2019"],
    "isPrivateFile" : false,
    "customCoordinates" : null,
    "fileType": "image"
}
```

### Understanding response

The JSON-encoded response contains details of the uploaded file which can have following properties:

| Field | Description |
| :--- | :--- |
| fileId | Unique fileId. Store this fileld in your database, as this will be used to perform update action on this file. |
| name | The name of the uploaded file. |
| url | The URL of the file. |
| thumbnailUrl | In case of an image, a small thumbnail URL. |
| height | Height of the uploaded image file. Only applicable when file type is image. |
| width | Width of the uploaded image file. Only applicable when file type is image. |
| size | Size of the uploaded file in bytes. |
| fileType | Type of file. It can either be `image` or `non-image`. |
| filePath | The path of the file uploaded. It includes any folder that you specified while uploading. |
| tags | Array of tags associated with the image. |
| isPrivateFile | Is the file marked as private. It can be either `true` or `false`. |
| customCoordinates | Value of custom coordinates associated with the image in format `x,y,width,height`. |
| metadata | The metadata of the upload file. Use `responseFields` property in request to get the metadata returned in response of upload API. |

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
    private_key='your_private_key',
    public_key='your_public_key',
    url_endpoint = 'your_url_endpoint'
)

upload = imagekit.upload(
    file=open("image.jpg", "rb"),
    file_name="my_file_name.jpg",
    options={
        "response_fields": ["is_private_file", "tags"],
        "tags": ["tag1", "tag2"]
    },
)

print("Upload binary", upload)
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
$uploadFile = $imageKit->upload(array(
    'file' => fopen(__DIR__."/image.jpg", "r"),
    'fileName' => "my_file_name.jpg",
    "tags" => implode(",", array("tag1", "tag2")),
    "customCoordinates" => implode(",", array("10", "10", "100", "100"))
));

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
upload = imagekitio.upload_file(file, "testing.jpg", {
    response_fields: 'tags,customCoordinates,isPrivateFile,metadata',
    tags: %w[abc def],
    use_unique_file_name: false,
    is_private_file: true
})
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
-F 'tags=t-shirt,summer,men'
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
    tags: ["t-shirt","summer","men"]
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
    private_key='your private_key',
    public_key='your public_key',
    url_endpoint = 'your url_endpoint'
)

with open("image.jpg", mode="rb") as img:
    imgstr = base64.b64encode(img.read())

upload = imagekit.upload(
    file=imgstr,
    file_name="my_file_name.jpg",
    options={
        "response_fields": ["is_private_file", "tags"],
        "tags": ["t-shirt", "summer","men"]
    },
)

print("Upload base64", upload)
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

$base64Img = "iVBORw0KGgoAAAAN";

// Upload Image - base64
$uploadFile = $imageKit->upload(array(
    'file' => $base64Img,
    'fileName' => "my_file_name.jpg",
    "tags" => implode(",", array("t-shirt", "summer", "men"))
));

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
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")

image64 = Base64.encode64(File.open("sample.jpg", "rb").read)

upload = imagekitio.upload_file(
    file = image64, file_name = "testing",
    options = {
        response_fields: 'tags,customCoordinates,isPrivateFile,metadata',
        tags: %w[abc def],
        use_unique_file_name: true
 })
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
    private_key='your private_key',
    public_key='your public_key',
    url_endpoint = 'your url_endpoint'
)

with open("image.jpg", mode="rb") as img:
    imgstr = base64.b64encode(img.read())

upload = imagekit.upload(
    file="https://imagekit.io/image.jpg",
    file_name="my_file_name.jpg",
    options={},
)

print("Upload url", upload)
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
$uploadFile = $imageKit->upload(array(
    'file' => "https://imagekit.io/image.jpg",
    'fileName' => "my_file_name.jpg"
));

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
    file = "image-url",
    file_name = "testing",
    options = {}
)
```
{% endtab %}
{% endtabs %}

