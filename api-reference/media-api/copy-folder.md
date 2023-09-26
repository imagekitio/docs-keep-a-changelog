# Copy folder

{% swagger baseUrl="https://api.imagekit.io" path="/v1/bulkJobs/copyFolder" method="post" summary="Copy folder API" %}
{% swagger-description %}
This will copy one folder into another.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceFolderPath" type="string" %}
The full path to the source folder you want to copy. For example - `/path/of/source/folder`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destinationPath" type="string" %}
Full path to the destination folder where you want to copy the source folder into. For example - `/path/of/destination/folder`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="includeFileVersions" type="boolean" %}
Option to copy all versions of files that are nested inside the selected folder. By default, only the current version of each file will be copied. When set to `true`, all versions of each file will be copied.

**Default value** \- `false`
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive a jobId which can be used to get the copy operation's status." %}
```javascript
{
    "jobId" : "598821f949c0a938d57563bd"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If no files or folders are found at the specified sourceFolderPath then a 404 response is returned." %}
```javascript
{
     "message" : "No files & folder found at sourceFolderPath /folder/to/copy",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "NO_FILES_FOLDER" 
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with JSON encoded response containing information about `jobId`. You can use `jobId` to get the status of this job using [bulk job status API](copy-move-folder-status.md). 

### Access control and permissions

Read how access and permissions are affected by this operation [here](../../media-library/overview/copy-and-move-folders.md#copy-folder).

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/bulkJobs/copyFolder" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFolderPath" : "/folder/to/copy",
	"destinationPath" : "/folder/to/copy/into/",
        "includeFileVersions": true
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

imagekit.copyFolder({
     sourceFolderPath: "/folder/to/copy",
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

copy_folder = imagekit.copy_folder(options=CopyFolderRequestOptions(source_folder_path='/source_folder_path',
                                                                    destination_path='/destination/path',
                                                                    include_file_versions=True))

print("Copy folder-", copy_folder, end="\n\n")

# Raw Response
print(copy_folder.response_metadata.raw)

# print the job's id
print(copy_folder.job_id)
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

$sourceFolderPath = "/folder/to/copy";
$destinationPath = "/folder/to/copy/into/";
$includeFileVersions = false;

$copyFolder = $imageKit->copyFolder([
    'sourceFolderPath' => $sourceFolderPath,
    'destinationPath' => $destinationPath,
    'includeFileVersions' => $includeFileVersions
]);

echo("Copy Folder : " . json_encode($copyFolder));
```
{% endtab %}

{% tab title="Java" %}
```java

CopyFolderRequest copyFolderRequest = new CopyFolderRequest();
copyFolderRequest.setSourceFolderPath("/folder/to/copy");
copyFolderRequest.setDestinationPath("/folder/to/copy/into/");
copyFolderRequest.setIncludeFileVersions(true);
ResultOfFolderActions resultOfFolderActions = ImageKit.getInstance().copyFolder(copyFolderRequest);

```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.copy_folder(
  source_folder_path: '/folder/to/copy',
  destination_path: '/folder/to/copy/into/',
  include_file_versions: false # optional
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.CopyFolder(ctx, media.CopyFolderParam{
    SourceFolderPath: "/folder/to/copy",
    DestinationPath: "/folder/to/copy/into/",
    IncludeVersions: false, // optional
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
CopyFolderRequest cpyFolderRequest = new CopyFolderRequest
{
    sourceFolderPath = "/folder/to/copy",
    destinationPath = "/folder/to/copy/into/",
    includeFileVersions = false // optional
};
ResultOfFolderActions resultOfFolderActions = imagekit.CopyFolder(cpyFolderRequest);
```
{% endtab %}

{% endtabs %}
