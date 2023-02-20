# Move folder

{% swagger baseUrl="https://api.imagekit.io" path="/v1/bulkJobs/moveFolder" method="post" summary="Move folder API" %}
{% swagger-description %}
This will move one folder into another. The selected folder, its nested folders, files, and their versions are moved in this operation. Note: If any file at the destination has the same name as the source file, then the source file and its versions will be appended to the destination file version history.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceFolderPath" type="string" %}
The full path to the source folder you want to move. For example - `/path/of/source/folder`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destinationPath" type="string" %}
Full path to the destination folder where you want to move the source folder into. For example - `/path/of/destination/folder`.
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive a jobId which can be used to get the move operation's status." %}
```javascript
{
    "jobId" : "598821f949c0a938d57563bd"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If no files or folders are found at specified sourceFolderPath then a 404 response is returned." %}
```javascript
{
     "message" : "No files & folder found at sourceFolderPath /folder/to/move",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "NO_FILES_FOLDER" 
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with JSON encoded response containing information about `jobId`. You can use `jobId` to get the status of this job using [bulk job status API](copy-move-folder-status.md). 

### Access control and permissions

Read how access and permissions are affected by this operation [here](../../media-library/overview/copy-and-move-folders.md#move-folder).

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/bulkJobs/moveFolder" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFolderPath" : "/folder/to/move",
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

imagekit.moveFolder({
     sourceFolderPath: "/folder/to/move",
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

move_folder = imagekit.move_folder(options=MoveFolderRequestOptions(source_folder_path="/demo1/testing",
                                                                    destination_path="/"))

print("Move folder-", move_folder, end="\n\n")

# Raw Response
print(move_folder.response_metadata.raw)

# print the job's id
print(move_folder.job_id)
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

$sourceFolderPath = '/folder/to/move';
$destinationPath = '/folder/to/move/into/';
$moveFolder = $imageKit->moveFolder([
    'sourceFolderPath' => $sourceFolderPath,
    'destinationPath' => $destinationPath
]);

echo("Move Folder : " . json_encode($moveFolder));
```
{% endtab %}

{% tab title="Java" %}
```java
MoveFolderRequest moveFolderRequest = new MoveFolderRequest();
moveFolderRequest.setSourceFolderPath("/folder/to/move");
moveFolderRequest.setDestinationPath("/folder/to/move/into/");
ResultOfFolderActions resultOfFolderActions = ImageKit.getInstance().moveFolder(moveFolderRequest);
```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.move_folder(source_folder_path: '/folder/to/move', destination_path: '/folder/to/move/into/')
```
{% endtab %%}

{% tab title='Go' %}
```go
resp, err := ik.Media.MoveFolder(ctx, media.MoveFolderParam{
    SourceFolderPath: "/folder/to/move",
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
MoveFolderRequest moveFolderRequest = new MoveFolderRequest
    {
        sourceFolderPath = "/folder/to/move",
        destinationPath = "/folder/to/move/into/"
    };
ResultOfFolderActions resultOfFolderActions1 = imagekit.MoveFolder(moveFolderRequest);
```
{% endtab %}

{% endtabs %}
