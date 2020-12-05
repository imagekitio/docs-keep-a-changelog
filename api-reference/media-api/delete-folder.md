# Delete folder

{% api-method method="delete" host="https://api.imagekit.io" path="/v1/folder/" %}
{% api-method-summary %}
Delete folder API
{% endapi-method-summary %}

{% api-method-description %}
This will delete the specified folder and all nested files & folders. This action is cannot be undone.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="folderPath" type="string" required=true %}
Full path to the folder you want to delete. For example `folder/to/delete/`
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
Folder deleted successfully
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If no folder is found at the specified `folderPath` then a `404` response is returned. 
{% endapi-method-response-example-description %}

```
{
     "message" : "No folder found with folderPath folder/to/delete/",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "FOLDER_NOT_FOUND" 
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

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
{% endtabs %}

