# Delete folder

{% swagger baseUrl="https://api.imagekit.io" path="/v1/folder/" method="delete" summary="Delete folder API" %}
{% swagger-description %}
This will delete the specified folder and all nested files, their versions & folders. This action cannot be undone.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="folderPath" type="string" %}
Full path to the folder you want to delete. For example `folder/to/delete/`.
{% endswagger-parameter %}

{% swagger-response status="204" description="Empty body is returned." %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="If no folder is found at the specified folderPath then a 404 response is returned. " %}
```javascript
{
     "message" : "No folder found with folderPath folder/to/delete/",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "FOLDER_NOT_FOUND" 
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `204` status code with an empty body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X DELETE "https://api.imagekit.io/v1/folder/" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"folderPath" : "folder/to/delete/"
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

imagekit.deleteFolder("folder/to/delete/", function(error, result) {
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

delete_folder = imagekit.delete_folder(options=DeleteFolderRequestOptions(folder_path="/test/demo"))

print("Delete folder-", delete_folder, end="\n\n")

# Raw Response
print(delete_folder.response_metadata.raw)
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

$folderPath = 'folder/to/delete/';
$deleteFolder = $imageKit->deleteFolder($folderPath);

echo("Delete Folder : " . json_encode($deleteFolder));
```
{% endtab %}

{% tab title="Java" %}
```java
DeleteFolderRequest deleteFolderRequest = new DeleteFolderRequest();
deleteFolderRequest.setFolderPath("folder/to/delete/");
ResultNoContent resultNoContent = ImageKit.getInstance().deleteFolder(deleteFolderRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.delete_folder(folder_path: 'folder/to/delete/')
```
{% endtab %}

[% tab title="Go" %}
```go
resp, err := ik.Media.DeleteFolder(ctx, media.DeleteFolderParam{
    FolderPath: "folder/to/delete/",
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
DeleteFolderRequest deleteFolderRequest = new DeleteFolderRequest
    {
        folderPath = "folder/to/delete/",
    };
ResultNoContent resultNoContent2 = imagekit.DeleteFolder(deleteFolderRequest);
```
{% endtab %}

{% endtabs %}
