# Copy file

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/copy" method="post" summary="Copy file API" %}
{% swagger-description %}
This will copy a file from one folder to another. Note: If any file at the destination has the same name as the source file, then the source file and its versions (if `includeFileVersions` is set to true) will be appended to the destination file version history.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceFilePath" type="string" %}
The full path of the file you want to copy. For example - `/path/to/file.jpg`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destinationPath" type="string" %}
Full path to the folder you want to copy the above file into. For example - `/folder/to/copy/into/`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="includeFileVersions" type="boolean" %}
Option to copy all versions of a file. By default, only the current version of the file is copied. When set to `true`, all versions of the file will be copied.

**Default value** \- `false`
{% endswagger-parameter %}

{% swagger-response status="204" description="Empty body is returned." %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="If no file is found at your sourceFilePath then a 404 response is returned." %}
```javascript
{
     "message" : "No file found with filePath /path/to/file.jpg",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "SOURCE_FILE_MISSING" 
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `204` status code with an empty body.

### Access control and permissions

Read how access and permissions are affected by this operation [here](../../media-library/overview/copy-and-move-files.md#copy-file).

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/files/copy" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFilePath" : "/path/to/file.jpg",
	"destinationPath" : "/folder/to/copy/into/",
    "includeFileVersions" : true
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

imagekit.copyFile({
     sourceFilePath: "/path/to/file.jpg",
     destinationPath: "/folder/to/copy/into/",
     includeFileVersions: false // optional
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

copy_file = imagekit.copy_file(options=CopyFileRequestOptions(source_file_path="/file.jpg",
                                       destination_path="/test",
                                       include_file_versions=True))

print("Copy file-", copy_file, end="\n\n")

# Raw Response
print(copy_file.response_metadata.raw)
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

$destinationPath = '/destination-folder';
$copyFile = $imageKit->copy([
    'sourceFilePath' => '/pah/to/file.jpg',
    'destinationPath' => '/folder/to/copy/into/',
    'includeFileVersions' => false
]);

echo("Copy File : " . json_encode($copyFile));
```
{% endtab %}

{% tab title="Java" %}
```java

CopyFileRequest copyFileRequest = new CopyFileRequest();
copyFileRequest.setSourceFilePath("/path/to/file.jpg");
copyFileRequest.setDestinationPath("/folder/to/copy/into/");
copyFileRequest.setIncludeFileVersions(true);
ResultNoContent resultNoContent = ImageKit.getInstance().copyFile(copyFileRequest);
```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.copy_file(
  source_file_path: '/pah/to/file.jpg',
  destination_path: '/folder/to/copy/into/',
  include_file_versions: false # optional
)
```
{% endtab %}

{% tab title='Go' %}
```go
resp, err := ik.Media.CopyFile(ctx, media.CopyFileParam{
    SourcePath: "/path/to/file.jpg",
    DestinationPath: "/folder/to/copy/into/",
    IncludeVersions: false, //optional
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
CopyFileRequest cpyRequest = new CopyFileRequest
    {
        sourceFilePath = "/path/to/file.jpg",
        destinationPath = "/folder/to/copy/into/"
    };
ResultNoContent resultNoContent = imagekit.CopyFile(cpyRequest);
```
{% endtab %}

{% endtabs %}
