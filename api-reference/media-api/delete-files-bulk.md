# Delete files (bulk)

You can programmatically delete multiple files uploaded in the media library using bulk file delete API.

{% hint style="info" %}
If a file or specific transformation has been requested in the past, then the response is cached. Deleting a file does not purge the cache. You can purge the cache using [purge API](purge-cache.md).
{% endhint %}

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/batch/deleteByFileIds" method="post" summary="Bulk file delete API" %}
{% swagger-description %}
Deletes multiple files and their versions from the media library.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fileIds" type="array" %}
Each value should be a unique fileId of the uploaded file. `fileId` is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, a 200 is returned along with the array of fileIds which are successfully deleted." %}
```javascript
{
    "successfullyDeletedFileIds": [
        "5e1c13d0c55ec3437c451406",
        ...
    ]
}
```
{% endswagger-response %}

{% swagger-response status="207" description="An array of fileIds is returned for successful and error cases on partial success." %}
```javascript
{
    "successfullyDeletedFileIds": [
        "5e1c13d0c55ec3437c451406",
        ...
    ],
    "errors": [
        {
            fileId: "3e21880d5efe355febd4bccx",
            error: "Error in deleting tags"
        },
        {
            fileId: "5fc1c55efe355febddcda3d1",
            error: "Error in deleting tags"
        }
    ]
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If any of the fileId is not found in your media library then a 404 response is returned and no file is deleted. The whole operation fails." %}
```javascript
{
    "message": "The requested file(s) does not exist.",
    "help": "For support kindly contact us at support@imagekit.io .",
    "missingFileIds": [
        "5e21880d5efe355febd4bccd",
        "5e1c13c1c55ec3437c451403"
    ]
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with JSON encoded response containing information about successfully deleted `fileIds`.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# Array of the unique fileId of the uploaded files. fileId is returned in response of list files API and upload API.
curl -X POST "https://api.imagekit.io/v1/files/batch/deleteByFileIds" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"fileIds" : ["file_id_1", "file_id_2"]
}
'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
imagekit.bulkDeleteFiles(["file_id_1","file_id_2"])
.then(response => {
    console.log(response);
})
.catch(error => {
    console.log(error);
});
```
{% endtab %}

{% tab title="Python" %}
```python
bulk_file_delete = imagekit.bulk_file_delete(file_ids=["file_id_1", "file_id_2"])
print("Bulk file delete-", bulk_file_delete, end="\n\n")

# Raw Response
print(bulk_file_delete.response_metadata.raw)

# list successfully deleted file ids
print(bulk_file_delete.successfully_deleted_file_ids)

# print the first file's id
print(bulk_file_delete.successfully_deleted_file_ids[0])
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

$fileIds = ["file_id_1", "file_id_2"];

$deleteBulkFiles = $imageKit->bulkDeleteFiles($fileIds);

echo("Delete Bulk files : " . json_encode($deleteBulkFiles));
```
{% endtab %}
{% tab title="Java" %}
```java
List<String> fileIds=new ArrayList<>();
fileIds.add("file_id_1");
fileIds.add("file_id_2");
ResultFileDelete result=ImageKit.getInstance().bulkDeleteFiles(fileIds);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
bulk_ids = Array["file_id_1","file_id_2"]
imagekitio.delete_bulk_files(file_ids: bulk_ids)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.DeleteBulkFiles(ctx, media.FileIdsParam{
    FileIds: []string{"file_id_1", "file_id_2"},
)
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
List<string> ob3 = newList<string>();
ob3.Add("fileId_1");
ob3.Add("fileId_2");
ResultFileDelete resultFileDelete = imagekit.BulkDeleteFiles(ob3);
```
{% endtab %}

{% endtabs %}
