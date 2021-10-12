# Copy folder

{% swagger baseUrl="https://api.imagekit.io" path="/v1/bulkJobs/copyFolder" method="post" summary="Copy folder API" %}
{% swagger-description %}
This will copy one folder into another.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of 

`your_private_api_key:`

\


Note the colon in the end
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceFolderPath" type="string" %}
The full path to the source folder you want to copy. For example - 

`/path/of/source/folder`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destinationPath" type="string" %}
Full path to the destination folder where you want to copy the source folder into. For example - 

`/path/of/destination/folder/`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive a jobId which can be used to get the copy operation's status." %}
```javascript
{
    "jobId" : "598821f949c0a938d57563bd"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="If no files or folders are found at the specified sourceFolderPath then a 404 response is returned." %}
```javascript
{
     "message" : "No files & folder found at sourceFolderPath /folder/to/copy",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "NO_FILES_FOLDER" 
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with JSON encoded response containing information about `jobId`. You can use `jobId` to get the status of this job using [bulk job status API](copy-move-folder-status.md). 

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/bulkJobs/copyFolder" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFolderPath" : "/folder/to/copy",
	"destinationPath" : "/folder/to/copy/into/"
}
'
```
{% endtab %}
{% endtabs %}



