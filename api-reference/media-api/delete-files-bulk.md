# Delete files \(bulk\)

You can programmatically delete multiple files uploaded in media library using bulk file delete API.

When you delete a file, all its transformations are also deleted. However, if a file or specific transformation has been requested in the past, then the response is cached in CDN. You can purge the cache from the CDN using [purge API](purge-cache.md).

{% api-method method="post" host="https://api.imagekit.io" path="/v1/files/batch/deleteByFileIds" %}
{% api-method-summary %}
Bulk file delete API
{% endapi-method-summary %}

{% api-method-description %}
Deletes multiple files and all their transformations.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-form-data-parameters %}
{% api-method-parameter name="fileIds" type="array" required=true %}
Each value should be a unique fileId of the uploaded file. `fileId` is returned in list files API and upload API
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
On success, a `200` is returned along with the array of `fileIds` which are successfully deleted.
{% endapi-method-response-example-description %}

```javascript
{
    "successfullyDeletedFileIds": [
        "5e1c13d0c55ec3437c451406",
        ...
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If any of the fileId in not found in your media library then a `404` response is returned and no file is deleted. The whole operation fails.
{% endapi-method-response-example-description %}

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
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with JSON encoded response containing information about successfully deleted `fileIds`.

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
	"fileIds" : ["5e1c2079c55ec3437c451b6d"]
}
'
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
imagekit.bulkDeleteFiles(["fileId1","fileId2"])
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
imagekit.bulk_file_delete(["file_id1", "file_id2"])
```
{% endtab %}

{% tab title="PHP" %}
```php
$imageKit->bulkFileDeleteByIds(array(
    "fileIds" => array("file_id_1", "file_id_2")
));
```
{% endtab %}

{% tab title="Java" %}
```java
List<String> fileIds=new ArrayList<>();
fileIds.add("fileId1");
fileIds.add("fileId2");
ResultFileDelete result=ImageKit.getInstance().bulkDeleteFiles(fileIds);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
bulk_ids = Array["fileId1","fileId2"]
imagekitio.bulk_file_delete(bulk_ids)
```
{% endtab %}
{% endtabs %}

