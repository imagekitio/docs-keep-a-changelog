# Copy file

{% api-method method="post" host="https://api.imagekit.io" path="/v1/files/copy" %}
{% api-method-summary %}
Copy file API
{% endapi-method-summary %}

{% api-method-description %}
This will copy a file from one folder to another.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="sourceFilePath" type="string" required=true %}
The full path of the file you want to copy. For example - `/path/to/file.jpg`
{% endapi-method-parameter %}

{% api-method-parameter name="destinationPath" type="string" required=true %}
Full path to the folder you want to copy the above file into. For example - `/folder/to/copy/into/`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If no file is found at your `sourceFilePath` then a `404` response is returned.
{% endapi-method-response-example-description %}

```javascript
{
     "message" : "No file found with filePath /path/to/file.jpg",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "SOURCE_FILE_MISSING" 
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `204` status code with an empty body.

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/files/copy" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFilePath" : "/path/to/file.jpg",
	"destinationPath" : "/folder/to/copy/into/"
}
'
```
{% endtab %}
{% endtabs %}

