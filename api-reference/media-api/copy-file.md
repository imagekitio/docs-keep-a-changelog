# Copy file

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/copy" method="post" summary="Copy file API" %}
{% swagger-description %}
This will copy a file from one folder to another.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of 

`your_private_api_key:`

\




**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceFilePath" type="string" %}
The full path of the file you want to copy. For example - 

`/path/to/file.jpg`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destinationPath" type="string" %}
Full path to the folder you want to copy the above file into. For example - 

`/folder/to/copy/into/`
{% endswagger-parameter %}

{% swagger-response status="204" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="If no file is found at your sourceFilePath then a 404 response is returned." %}
```javascript
{
     "message" : "No file found with filePath /path/to/file.jpg",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "SOURCE_FILE_MISSING" 
}
```
{% endswagger-response %}
{% endswagger %}

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
