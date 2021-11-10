# Delete folder

{% swagger baseUrl="https://api.imagekit.io" path="/v1/folder/" method="delete" summary="Delete folder API" %}
{% swagger-description %}
This will delete the specified folder and all nested files & folders. This action is cannot be undone.
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
```json
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
{% endtabs %}
