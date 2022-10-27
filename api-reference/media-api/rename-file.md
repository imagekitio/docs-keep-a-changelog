# Rename file

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/rename" method="put" summary="Rename file API" %}
{% swagger-description %}
You can programmatically rename an already existing file in the media library using rename file API. This operation would rename all file versions of the file. Note: The old URLs will stop working. The file/file version URLs cached on CDN will continue to work unless a purge is requested.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filePath" type="string" %}
The full path of the file you want to rename. For example - `/path/to/file.jpg`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="newFileName" type="string" %}
The new name of the file. A filename can contain:

\- Alphanumeric Characters: `a-z`, `A-Z`, `0-9` (including Unicode letters, marks, and numerals in other languages). 

\- Special Characters: `.`, `_`, and `-`.

Any other character, including space, will be replaced by `_`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="purgeCache" type="boolean" %}
Option to purge cache for the old file URL. When set to `true`, it will internally issue a purge cache request on CDN to remove cached content on the old URL. E.g. if an old file was accessible at  - `https://ik.imagekit.io/demo/old-filename.jpg`, a purge cache request will be issued to remove the CDN cache for this URL. This purge request is counted against your monthly purge quota. Note: Cache will be purged only for the current version of file.

**Default value** \- `false`
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive purgeRequestId in the response body, which can be used to get the purge request status. This is only sent if the purgeCache is set to true in the request. Otherwise, the response is an empty JSON." %}
```javascript
// When purgeCache is set to true
{
    purgeRequestId: "598821f949c0a938d57563bd"
}

// When purgeCache is not set or set to false, response is empty
{}
```
{% endswagger-response %}

{% swagger-response status="207" description="In case purgeCache is set to true and total purge request count has exceeded the quota, we will rename the file but won't purge CDN cache." %}
```javascript
{
    "message" : "File renamed successfully but we could not purge the CDN cache for old URL because of rate limits on purge API.",
    "help" : "For support kindly contact us at support@imagekit.io .",
    "reason" : "PURGE_FAILED" 
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If no file is found at the specified filePath in the media library, then a 404 response is returned." %}
```javascript
{
     "message" : "No file found in media library at filePath /path/to/file.jpg",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "FILE_MISSING" 
}
```
{% endswagger-response %}

{% swagger-response status="409" description="If a file with newFileName already exist in the same location, a 409 response is returned." %}
```javascript
{
     "message" : "File with name newFileName already exists at the same location.",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "FILE_ALREADY_EXISTS" 
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the JSON-encoded response body.

The response depends upon the value of `purgeCache` in the request. If `purgeCache` is set to `true`, the response contains `purgeRequestId`, which can be used to get the [purge cache status](purge-cache-status.md).

{% tabs %}
{% tab title="purgeCache=true" %}
```javascript
// When purgeCache is set to true, we send the purge request ID.
// You can use this to get purge status
{
    purgeRequestId: "598821f949c0a938d57563bd"
}
```
{% endtab %}

{% tab title="purgeCache=false or unset" %}
```javascript
// When purgeCache is set to false or not set
{}
```
{% endtab %}
{% endtabs %}

### Example

Here is an example request to understand API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X PUT "https://api.imagekit.io/v1/files/rename" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"filePath" : "/path/to/old-file-name.jpg",
	"newFileName" : "new-file-name.jpg",
	"purgeCache": false
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

imagekit.renameFile({
     filePath: "/path/to/old-file-name.jpg",
     newFileName: "new-file-name.jpg",
     purgeCache: false // optional
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

rename_file = imagekit.rename_file(options=RenameFileRequestOptions(file_path="/file_path.jpg",
                                                                    new_file_name="new_file_name.jpg",
                                                                    purge_cache=True))

print("Rename file-", rename_file, end="\n\n")

# Raw Response
print(rename_file.response_metadata.raw)

# print the purge request id
print(rename_file.purge_request_id)
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

$filePath='/path/to/old-file-name.jpg';
$newFileName = 'new-file-name.jpg';
$renameFile = $imageKit->rename([
    'filePath' => $filePath,
    'newFileName' => $newFileName,
    'purgeCache' => false,  // optional
]);

echo("Rename File : " . json_encode($renameFile));
```
{% endtab %}

{% tab title="Java" %}
```java
RenameFileRequest renameFileRequest = new RenameFileRequest();
renameFileRequest.setFilePath("/path/to/old-file-name.jpg");
renameFileRequest.setNewFileName("new-file-name.jpg");
renameFileRequest.setPurgeCache(false);
ResultRenameFile resultRenameFile = ImageKit.getInstance().renameFile(renameFileRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.rename_file(
  file_path: '/path/to/old-file-name.jpg',
  new_file_name: 'new-file-name.jpg',
  purge_cache: false #optional
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.RenameFile(ctx, media.RenameFileParam{
    FilePath: "/path/to/old-file-name.jpg",
    NewFileName: "new-file-name.jpg",
    PurgeCache: false, // Optional
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
RenameFileRequest renameFileRequest = new RenameFileRequest
    {
        filePath = "path_1",
        newFileName = "file_name",
        purgeCache = false
    };
ResultRenameFile resultRenameFile = imagekit.RenameFile(renameFileRequest);
```
{% endtab %}

{% endtabs %}
