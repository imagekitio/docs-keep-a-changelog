# Add tags \(bulk\)

{% api-method method="post" host="https://api.imagekit.io" path="/v1/files/addTags" %}
{% api-method-summary %}
Add tags in bulk API
{% endapi-method-summary %}

{% api-method-description %}
Add tags to multiple files in a single request.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter type="string" name="Authorization" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter type="array" name="fileIds" required=true %}
Each value should be a unique `fileId` of the uploaded file. `fileId` is returned in list files API and upload API
{% endapi-method-parameter %}

{% api-method-parameter name="tags" type="array" required=true %}
An array of tags to add on these files.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If any of the fileId is not found in your media library then a `404` response is returned and no tags are added to any file. The whole operation fails.
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

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with an empty body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/files/addTags" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"fileIds" : [
		"5e21880d5efe355febd4bccd",
		"5e1c13c1c55ec3437c451403",
		"5f4abf6fae77ae7f0acda3d1", 
		"5f207bd1bd2741182ceadd55"
	],
	"tags" : [
		"tag-to-add-1", 
		"tag-to-add-2"
	]
}
'
```
{% endtab %}
{% endtabs %}

