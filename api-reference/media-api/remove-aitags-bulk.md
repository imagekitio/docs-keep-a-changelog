# Remove AITags (bulk)

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/removeAITags" method="post" summary="Remove AITags in bulk API" %}
{% swagger-description %}
Remove AITags from multiple files in a single request.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fileIds" type="array" %}
Each value should be a unique `fileId` of the uploaded file. `fileId` is returned in the list files API and upload API. `AITags` can only be removed from the current version of the file.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AITags" type="array" %}
An array of `AITags` to remove from these files.
{% endswagger-parameter %}

{% swagger-response status="200" description="An array of fileIds from which specified AITags were successfully removed." %}
```javascript
{
    "successfullyUpdatedFileIds": [
        "5e21880d5efe355febd4bccd",
        "5e1c13c1c55ec3437c451403",
        "5f4abf6fae77ae7f0acda3d1", 
        "5f207bd1bd2741182ceadd55"
    ]
}
```
{% endswagger-response %}

{% swagger-response status="207" description="An array of fileIds is returned for successful and error cases on partial success." %}
```javascript
{
    "successfullyUpdatedFileIds": [
        "5e21880d5efe355febd4bccd",
        "5e1c13c1c55ec3437c451403",
        "5f4abf6fae77ae7f0acda3d1", 
        "5f207bd1bd2741182ceadd55"
    ],
    "errors": [
        {
            fileId: "3e21880d5efe355febd4bccx",
            error: "Error in removing AITags"
        },
        {
            fileId: "5fc1c55efe355febddcda3d1",
            error: "Error in removing AITags"
        }
    ]
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If any fileId is not found in your media library then a 404 response is returned and no AITags are removed from any file. The whole operation fails." %}
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

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with an array of successfully updated `fileIds`.

### Examples

Here is an example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/files/removeAITags" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"fileIds" : [
		"file_id_1",
		"file_id_2"
	],
	"AITags" : [
		"ai_tag_to_remove_1", 
		"ai_tag_to_remove_2"
	]
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

var fileIds = ["file_id_1", "file_id_2"];
var tags = ["ai_tag_to_remove_1", "ai_tag_to_remove_2"];

imagekit.bulkRemoveAITags(fileIds, tags, function(error, result) {
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

remove_ai_tags = imagekit.remove_ai_tags(file_ids=['file-id-1', 'file-id-2'], ai_tags=['remove-ai-tag-1', 'remove-ai-tag-2'])

print("Remove AI tags-", remove_ai_tags, end="\n\n")

# Raw Response
print(remove_ai_tags.response_metadata.raw)

# list successfully updated file ids
print(remove_ai_tags.successfully_updated_file_ids)

# print the first file's id
print(remove_ai_tags.successfully_updated_file_ids[0])
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

$fileIds = [
	"file_id_1",
	"file_id_2"
];

$AITags = [
	"ai_tag_to_remove_1", 
	"ai_tag_to_remove_2"
];

$bulkRemoveAITags = $imageKit->bulkRemoveAITags($fileIds, $AITags);

echo("Remove AI Tags (Bulk) : " . json_encode($bulkRemoveAITags));
```
{% endtab %}

{% tab title="Java" %}
```java
List<String> fileIds = new ArrayList<>();
fileIds.add("file_id_1");
fileIds.add("file_id_2");
List<String> aiTags = new ArrayList<>();
aiTags.add("ai_tag_to_remove_1");
aiTags.add("ai_tag_to_remove_2");
AITagsRequest aiTagsRequest = new AITagsRequest();
aiTagsRequest.setFileIds(fileIds);
aiTagsRequest.setAITags(aiTags);
ResultTags resultTags = ImageKit.getInstance().removeAITags(aiTagsRequest);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.delete_bulk_ai_tags(
  file_ids:  [
        "file_id_1",
        "file_id_2"
    ],
    ai_tags: [
        "ai_tag_to_remove_1", 
        "ai_tag_to_remove_2"
    ]
)
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Media.RemoveAITags(ctx, media.AITagsParam{
    FileIds: []string{
        "file_id_1",
        "file_id_2",
    },
    AITags: []string{"ai_tag_to-remove_1", "ai_tag_to-remove_2"},
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
 AITagsRequest removeAITagsRequest = new AITagsRequest
    {
        AITags = new List<string>
        {
            "tag_1",
            "tag_2"
        },
        fileIds = new List<string>
        {
            "fileId_1",
        },
};
ResultTags removeAITags = imagekit.RemoveAITags(removeAITagsRequest);
```
{% endtab %}

{% endtabs %}
