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
```bash
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
		"5e21880d5efe355febd4bccd",
		"5e1c13c1c55ec3437c451403",
		"5f4abf6fae77ae7f0acda3d1", 
		"5f207bd1bd2741182ceadd55"
	],
	"AITags" : [
		"ai-tag-to-remove-1", 
		"ai-tag-to-remove-2"
	]
}
'
```
{% endtab %}
{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.delete_bulk_ai_tags(
  file_ids:  [
    "5e21880d5efe355febd4bccd",
    "5e1c13c1c55ec3437c451403",
    "5f4abf6fae77ae7f0acda3d1",
    "5f207bd1bd2741182ceadd55"
    ],
    ai_tags: ['ai-tag-to-remove-1', 'ai-tag-to-remove-2']
)
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
    	"5e21880d5efe355febd4bccd",
    	"5e1c13c1c55ec3437c451403",
    	"5f4abf6fae77ae7f0acda3d1", 
    	"5f207bd1bd2741182ceadd55"
    ];

$AITags = [
		"ai-tag-to-remove-1", 
		"ai-tag-to-remove-2"
	];

$bulkRemoveAITags = $imageKit->bulkRemoveAITags($fileIds, $tags);

echo("Remove AI Tags (Bulk) : " . json_encode($bulkRemoveAITags));
```
{% endtab %}
{% endtabs %}
