# Move file

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/move" method="post" summary="Move file API" %}
{% swagger-description %}
This will move a file and all its versions from one folder to another. Note: If any file at the destination has the same name as the source file, then the source file and its versions will be appended to the destination file.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceFilePath" type="string" %}
The full path of the file you want to move. For example - `/path/to/file.jpg`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destinationPath" type="string" %}
Full path to the folder you want to move the above file into. For example - `/folder/to/move/into/`
{% endswagger-parameter %}

{% swagger-response status="204" description="Empty body is returned." %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="If no file is found at the specified sourceFilePath then a 404 response is returned." %}
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

Read how access and permissions are affected by this operation [here](../../media-library/overview/copy-and-move-files.md#move-file).

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/files/move" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFilePath" : "/path/to/file.jpg",
	"destinationPath" : "/folder/to/move/into/"
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

imagekit.moveFile({
     sourceFilePath: "/path/to/file.jpg",
     destinationPath: "/folder/to/move/into/"
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

move_file = imagekit.move_file(options=MoveFileRequestOptions(source_file_path="/file.jpg",
                                                              destination_path="/test"))

print("Move file-", move_file, end="\n\n")

# Raw Response
print(move_file.response_metadata.raw)
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

$sourceFilePath = '/path/to/file.jpg';
$destinationPath = '/folder/to/move/into/';
$moveFile = $imageKit->move([
    'sourceFilePath' => $sourceFilePath,
    'destinationPath' => $destinationPath
]);

echo("Move File : " . json_encode($moveFile));
```
{% endtab %}

{% tab title="Java" %}
```java
MoveFileRequest moveFileRequest = new MoveFileRequest();
moveFileRequest.setSourceFilePath("/path/to/file.jpg");
moveFileRequest.setDestinationPath("/folder/to/move/into/");
ResultNoContent resultNoContent = ImageKit.getInstance().moveFile(moveFileRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.move_file(
  source_file_path: '/path/to/file.jpg',
  destination_path: '/folder/to/move/into/'
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.MoveFile(ctx, media.MoveFileParam{
    SourcePath: "/path/to/file.jpg",
    DestinationPath: "/folder/to/move/into/",
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
MoveFileRequest moveFile = new MoveFileRequest
    {
        sourceFilePath = "/path/to/file.jpg",
        destinationPath = "/folder/to/copy/into/"
    };
ResultNoContent resultNoContentMoveFile = imagekit.MoveFile(moveFile);
```
{% endtab %}

{% endtabs %}
